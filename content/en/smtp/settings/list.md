---
title: "Local Lists"
description: ""
date: 2022-08-28T12:49:20Z
lastmod: 2022-08-28T12:49:20Z
draft: false
images: []
type: docs
menu:
  smtp:
    parent: "settings"
    identifier: "lists"
weight: 205
toc: true
---

## Overview

Lists are collections of values that are used primarily for lookup operations, such as through the `in-list` rule comparator. Lists can either be defined directly in the configuration file or read from an external file. To use an external file as a list, it must contain one entry per line and must be referenced in the configuration file using the `file://` prefix followed by the file location.

The definition of a list in the configuration file is done using the `list.<id>` key, where the value is an array of strings or file path URLs. For example:

```txt
[list]
domains = ["foobar.org", "foobar.net", "foobar.com"]
users = "file:///usr/local/stalwart-smtp/etc/users.txt"
blocked_ips = [ "file:///usr/local/stalwart-smtp/etc/blocked_ips_v4.txt", 
                "file:///usr/local/stalwart-smtp/etc/blocked_ips_v6.txt" ]
```

## Lookups

Lists can be referenced by using `list/<id>` in a lookup attribute and used in rules with the `in-list = "list/<id>"` comparator to check if a value is contained in the list. For example:

```txt
[session.rcpt.lookup]
domains = "list/domains"
```

While it is not recommended, lists can also be utilized for authentication lookups by including the username and password separated by a colon in the list, for example:

```txt
[list]
paid_clients = [ "john:his_secret_pass", "jane:s3cr3t", "bill:123456" ]

[session.auth]
lookup = [ { if = "listener", ne = "smtp", then = "list/paid_clients" }, 
           { else = false } ]
```

