---
title: "Transport & Routing"
description: ""
date: 2022-08-28T12:49:20Z
lastmod: 2022-08-28T12:49:20Z
draft: false
images: []
type: docs
menu:
  smtp:
    parent: "outbound"
    identifier: "transport"
weight: 402
toc: true
---

## Routing

Stalwart SMTP allows to define how messages should be delivered to their final destination through routing rules. Rules are configured under the `queue.outbound.next-hop` parameter, which can either point to a [remote host](/smtp/settings/remote) defined in the configuration file or contain the value `false` which indicates that the message delivery should be done through DNS resolution. Routing rules are useful for tasks such as forwarding messages for local domains to a message store over LMTP.

Example:

```txt
[queue.outbound]
next-hop = [ { if = "rcpt-domain", in-list = "list/domains", then = "lmtp" }, 
             { else = false } ]

[remote."lmtp"]
address = "localhost"
port = 24
protocol = "lmtp"
concurrency = 10
timeout = "1m"

[remote."lmtp".tls]
implicit = false
allow-invalid-certs = true

[list]
domains = ["foobar.net", "foobar.org"]
```

## IP Strategy

### Remote IP

The remote IP strategy determines which type of IP address to use when delivering emails to a remote SMTP server. The IP strategy is configured with the `queue.outbound.ip-strategy` parameter and can be set to use only IPv4 addresses, only IPv6 addresses, or both IPv4 and IPv6 addresses:

- `ipv4-only`: Use only IPv4 addresses.
- `ipv6-only`: Use only IPv6 addresses.
- `ipv6-then-ipv4`: Try first using IPv6 addresses and fallback to IPv4 if unavailable.
- `ipv4-then-ipv6`: Try first using IPv4 addresses and fallback to IPv6 if unavailable.

Example:

```txt
[queue.outbound]
ip-strategy = "ipv4-then-ipv6"
```

### Source IP

The source IP strategy determines a list of local IPv4 and IPv6 addresses to use when delivery emails to remote SMTP servers. If multiple source addresses are provided, Stalwart SMTP will randomly choose one from the list each time a new connection is established. The list of local IPv4 addresses to use is configured with the `queue.outbound.source-ip.v4` parameter while IPv6 addresses are configured under the `queue.outbound.source-ip.v6` parameter.

Example:

```txt
[queue.outbound.source-ip]
v4 = ["10.0.0.10", "10.0.0.11"]
v6 = ["a::b", "a::c"]
```

## Transport

### EHLO hostname

The `queue.outbound.hostname` parameter indicates which EHLO hostname to use when sending messages to remote SMTP servers. If this parameter is missing, the hostname specified in `server.hostname` is used instead.

Example:

```txt
[queue.outbound]
hostname = "mx.mydomain.net"
```

### Limits

The following transport limits can be configured under the `queue.outbound.limits` key:

- `mx`: The maximum number of MX hosts to try on each delivery attempt.
- `multihomed`: For multi-homed remote servers, it is the maximum number of IP addresses to try on each delivery attempt.

Example:

```txt
[queue.outbound.limits]
mx = 7
multihomed = 2
```

### Timeouts

Timeout options determine the time limit for the SMTP server to complete a specific step in the SMTP transaction process. These following timeout settings can defined in the configuration file under the `queue.outbound.timeouts`:

- `connect`: The maximum time to wait for the connection to be established.
- `greeting`: The maximum time to wait for the SMTP server's greeting message.
- `tls`: The maximum time to wait for the TLS negotiation process.
- `ehlo`: The maximum time to wait for the `EHLO` command.
- `mail-from`: The maximum time to wait for the `MAIL FROM` command.
- `rcpt-to`: The maximum time to wait for the `RCPT TO` command.
- `data`: The maximum time to wait for the `DATA` command.
- `mta-sts`: The maximum time to wait for a MTA-STS policy lookup.

Example:

```txt
[queue.outbound.timeouts]
connect = "3m"
greeting = "3m"
tls = "2m"
ehlo = "3m"
mail-from = "3m"
rcpt-to = "3m"
data = "10m"
mta-sts = "2m"
```
