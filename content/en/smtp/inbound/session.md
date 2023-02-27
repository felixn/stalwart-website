---
title: "SMTP Session"
description: ""
date: 2022-08-28T12:49:20Z
lastmod: 2022-08-28T12:49:20Z
draft: false
images: []
type: docs
menu:
  smtp:
    parent: "inbound"
    identifier: "session"
weight: 302
toc: true
---

## Sessions

A session in Stalwart SMTP refers to an incoming SMTP connection, and the configuration file has a dedicated section for each stage of the SMTP session (connect, EHLO, AUTH, MAIL, RCPT, and DATA). Most stages of an SMTP session support the use of [Sieve scripts](/smtp/inbound/sieve), which enable the creation of custom filters and modification of incoming message contents.

## Limits

The limits for an SMTP session can be set under the session key with the following attributes:

- `timeout`: The time limit for inactivity before a session is terminated.
- `transfer-limit`: The maximum number of bytes that can be transmitted during a single SMTP session.
- `duration`: The maximum length of time for an SMTP session.

Example:

```txt
[session]
timeout = "5m"
transfer-limit = 262144000 # 250 MB
duration = "10m"
```

## Connect stage

Currently, the only configuration option available for the connect stage is the `session.connect.script` attribute, which specifies the name of the [Sieve script](/smtp/inbound/sieve) to run before the SMTP session begins. This can be useful for filtering connections based on their remote IP address, for example.

Example:

```txt
[session.connect]
script = "connect_filter"

[sieve.scripts]
connect_filter = '''
require ["variables", "reject"];

if string "${env.remote_ip}" "10.0.0.88" {
    reject "Your IP '${env.remote_ip}' is not welcomed here.";
}
'''
```
