---
title: "Databases"
description: ""
date: 2022-08-28T12:49:20Z
lastmod: 2022-08-28T12:49:20Z
draft: false
images: []
type: docs
menu:
  smtp:
    parent: "settings"
    identifier: "databases"
weight: 204
toc: true
---

## Overview

The databases section of the configuration file enables the setup of SQL servers for user authentication and entry lookup purposes. At present, Stalwart SMTP supports PostgreSQL, MySQL, MSSQL and SQLite databases

## Database settings

SQL databases are defined in the configuration file using the `database.<id>` key and have the following parameters available:

- `address`: The URL of the SQL database, formatted as `<db_type>://[<username>:<secret>]<location>/<params>`. Supported database types are `postgres`, `mysql`, `mssql` and `sqlite`.
- `max-connections`: The maximum number of concurrent connections to the database (defaults to `10`).
- `min-connections`: The minimum number of open connections to keep open at all times (defaults to `0`).
- `idle-timeout`: The number of seconds before an idle SQL connection is closed (defaults to no timeout).

The connection details for the database must be specified within the address parameter. For instance, to connect to a PostgreSQL server located at `db.mydomain.net`, access the `clients` database, and use the credentials `smtp` and `mypass`, the address value should be set as follows:

```txt
address = "postgres://smtp:mypass@db.mydomain.net/clients"
```

Or, when using a local SQLite database at `/usr/local/stalwart-smtp/etc/sqlite.db`:

```txt
address = "sqlite:///usr/local/stalwart-smtp/etc/sqlite.db?mode=rwc"
```

## Lookups

The lookups section in the configuration file specifies a list of SQL queries to be used for validating recipients, authenticating accounts, verifying addresses, expanding mailing lists or checking the presence of a value in a table. The SQL statements for each lookup key are defined as `database.<id>.lookup.<query_id>` keys and can be referenced by using `db/<id>/<query_id>` in a lookup attribute or executed from rules through the `in-list = "list/<id>"` comparator.

For example, the lookup `trusted_ips` can be defined as:

```txt
[database."mysql".lookup]
trusted_ips = "SELECT EXISTS(SELECT 1 FROM trusted_ips WHERE address=? LIMIT 1)"
```

And then referenced from a rule to determine which remote hosts are allowed to relay:

```txt
relay = [ { any-of = [ { if = "remote-ip", in-list = "db/mysql/trusted_ips" },
                       { if = "authenticated-as", ne = "" }], then = true }, 
          { else = false } ]
```

### Caching

In order to reduce the number of requests made to a database, it is possible to enable caching for one or multiple lookup queries. Stalwart SMTP uses a Least Recently Used (LRU) caching strategy and maintains separate positive and negative caches for each query. Successful lookups are stored in the positive cache while failed or non-existent lookups are stored in the negative cache. The lookup cache is configured through the following parameters located under the `database.<id>.cache` key in the configuration file:

- `enable`: An array listing the lookup query ids to enable caching for (defaults to none).
- `entries`: Specifies the maximum number of entries that the LRU cache can hold (defaults to `1024`).
- `ttl.positive`: The time-to-live or number of seconds that a positive cached entry will remain valid (defaults to `1d`).
- `ttl.negative:` The time-to-live or number of seconds that a negative cached entry will remain valid (defaults to `1h`).

## Examples

The following example defines SQLite and PostgreSQL databases:

```txt
[database."sqlite"]
address = "sqlite:///usr/local/stalwart-smtp/etc/sqlite.db?mode=rwc"

[database."postgres"]
address = "postgres://postgres:password@localhost/stalwart"
max-connections = 10
min-connections = 0
idle-timeout = "5m"

[database."postgres".lookup]
auth = "SELECT secret FROM users WHERE email=?"
rcpt = "SELECT EXISTS(SELECT 1 FROM users WHERE email=? LIMIT 1)"
vrfy = "SELECT email FROM users WHERE email LIKE '%' || ? || '%' LIMIT 5"
expn = "SELECT member FROM mailing_lists WHERE id = ?"
domains = "SELECT EXISTS(SELECT 1 FROM domains WHERE name=? LIMIT 1)"

[database."postgres".cache]
enable = ["rcpt", "domains"]
entries = 1000
ttl = {positive = "1d", negative = "1h"}
```