---
title: "Remote hosts"
description: ""
date: 2022-08-28T12:49:20Z
lastmod: 2022-08-28T12:49:20Z
draft: false
images: []
type: docs
menu:
  smtp:
    parent: "settings"
    identifier: "remote"
weight: 203
toc: true
---

## Overview

Remote hosts enable the definition of external SMTP, LMTP, and IMAP4 servers for the purpose of relaying messages or authenticating users. These remote servers are specified in the configuration file using the `remote.<id>` keys, where `<id>` is the internal identifier for the host.

## Host Settings

For each remote host, the following parameters can be specified as sub-keys of `remote.<id>`:

- `address`: The fully-qualified domain name of the remote server, for example "mail.domain.org".
- `port`: The port on the remote server that should be used for the connection.
- `protocol`: The communication protocol to use, with valid options being `lmtp`, `smtp`, or `imap`.
- `concurrency`: The maximum number of open connections to the remote host that can be established at any given time (default is `10`).
- `timeout`: The time limit for establishing a connection to the remote host (default is `60s`).
- `lookup`: A boolean value indicating whether this host can be used for looking up accounts (default is `false`).

### TLS Options

The configuration of TLS for remote servers is managed through the following parameters, which are located under the `remote.<id>.tls` key in the configuration file:

- `implicit`: Specifies whether TLS should be implicitly established upon connecting to the remote host, or if it should be negotiated on a clear-text connection using the `STARTTLS` command (defaults to `true`).
- `allow-invalid-certs`: A flag indicating whether self-signed and invalid certificates should be accepted (defaults to `false`).

### Authentication

For remote SMTP and LMTP hosts, authentication can be configured using the following parameters, which are located under the `remote.<id>.auth` key in the configuration file:

- `username`: The username to be used for authentication.
- `secret`: The password to be used for authentication.

### Limits

The connection limits for a remote host can be set through the following parameters located under the `remote.<id>.limits` key in the configuration file:

- `errors`: Specifies the maximum number of errors before a connection from the pool is closed (defaults to `3`).
- `requests`: Specifies the maximum number of sequential requests that can be sent on each connection (defaults to `50`). This parameter is used to ensure compatibility with remote hosts that only allow a limited number of commands per connection.

## Lookups

Lookups allow the utilization of a remote host for user authentication, mailbox validation, address verification, mailing list expansion, and address existence checking in rules using the `in-list` comparator. By default, lookups are disabled for remote hosts, but can be enabled by setting the `remote.<id>.lookup` parameter to `true`.

When the `lookup` setting is enabled for a remote IMAP host, it can be utilized for user authentication purposes. For remote LMTP or SMTP hosts with `lookup` enabled, the server can be employed for recipient validation (`RCPT TO`), authentication (`AUTH`), address verification (`VRFY`), list expansion (`EXPN`), as well as in [rules](/settings/rules) using the `in-list = "remote/<id>"` comparator.

### Caching

To reduce the amount of lookup requests to a remote host, it is recommended to enable caching. Stalwart SMTP uses a Least Recently Used (LRU) caching strategy and maintains separate positive and negative caches for each host. Successful lookups are stored in the positive cache while failed or non-existent lookups are stored in the negative cache. The lookup cache is configured through the following parameters located under the `remote.<id>.cache` key in the configuration file:

- `entries`: Specifies the maximum number of entries that the LRU cache can hold (defaults to `1024`).
- `ttl.positive`: The time-to-live or number of seconds that a positive cached entry will remain valid (defaults to `1d`).
- `ttl.negative:` The time-to-live or number of seconds that a negative cached entry will remain valid (defaults to `1h`).

## Examples

### LMTP server

```txt
[remote."lmtp"]
address = "localhost"
port = 24
protocol = "lmtp"
concurrency = 10
timeout = "1m"
lookup = true

[remote."lmtp".cache]
entries = 1000
ttl = {positive = "1d", negative = "1h"}

[remote."lmtp".tls]
implicit = false
allow-invalid-certs = true

[remote."lmtp".auth]
username = "relay_server"
secret = "123456"

[remote."lmtp".limits]
errors = 3
requests = 50
```

### IMAP server

```txt
[remote."imap"]
address = "mail.mydomain.org"
port = 143
protocol = "imap"
concurrency = 10
timeout = "1m"
lookup = true

[remote."imap".cache]
entries = 1000
ttl = {positive = "1d", negative = "1h"}

[remote."imap".tls]
implicit = false
allow-invalid-certs = true

```
