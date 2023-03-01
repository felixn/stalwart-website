---
title: "Stalwart SMTP Server Documentation"
description: ""
lead: ""
date: 2022-01-25T14:40:56+01:00
lastmod: 2022-01-25T14:40:56+01:00
draft: false
images: []
type: docs
---

## Stalwart SMTP Server

Stalwart SMTP is a modern SMTP server developed in Rust with a focus on security, speed, and extensive configurability. 
It features built-in DMARC, DKIM, SPF and ARC support for message authentication, strong transport security through DANE, MTA-STS and SMTP TLS reporting, and offers great flexibility and customization thanks to its dynamic configuration rules and native support for Sieve scripts.

Key features:

- Sender and Message Authentication:
  - Domain-based Message Authentication, Reporting, and Conformance (**DMARC**) verification and failure/aggregate reporting.
  - DomainKeys Identified Mail (**DKIM**) verification, signing and failure reporting.
  - Sender Policy Framework (**SPF**) policy evaluation and failure reporting.
  - Authenticated Received Chain (**ARC**) verification and sealing.
  - Reverse IP (**iprev**) validation.
- Strong Transport Security:
  - DNS-Based Authentication of Named Entities (**DANE**) Transport Layer Security.
  - SMTP MTA Strict Transport Security (**MTA-STS**).
  - SMTP TLS Reporting (**TLSRPT**) delivery and analysis.
- Inbound Filtering and Throttling:
  - Sieve scripting language with support for all [registered extensions](https://www.iana.org/assignments/sieve-extensions/sieve-extensions.xhtml).
  - Filtering, modification and removal of MIME parts or headers.
  - DNS block lists (**DNSBL**) & Greylisting.
  - Inbound concurrency & rate limiting.
  - Integration with external content filtering systems such as SpamAssassin and ClamAV.
- Flexible Queues:
  - Unlimited virtual queues with custom routing rules.
  - Delayed delivery with `FUTURERELEASE` and `DELIVERBY` extensions support.
  - Priority delivery with `MT-PRIORITY` extension support.
  - Outbound throttling & Disk quotas.
- Logging and Reporting:
  - Detailed logging of SMTP transactions and events, including delivery attempts, errors, and policy violations.
  - Integration with **OpenTelemetry** to enable monitoring, tracing, and performance analysis of SMTP server operations.
  - Automatic analysis of incoming DMARC/TLS aggregate reports, DMARC/DKIM/SPF authentication failure reports as well as abuse reports.
- And more:
  - SASL authentication.
  - PostgreSQL, MySQL, MSSQL and SQLite support.
  - Granular configuration rules.
  - REST API for management.
  - Memory safe (thanks to Rust).

## Get Started

Install Stalwart SMTP on your server by following the instructions for your platform:

- [Linux / MacOS](/smtp/get-started/linux/)
- [Windows](/smtp/get-started/windows/)
- [Docker](/smtp/get-started/docker/)

You may also [compile Stalwart SMTP from the source](/smtp/development/compile/).

## Support

If you are having problems running Stalwart SMTP, you found a bug or just have a question,
do not hesitate to reach us on [Github Discussions](https://github.com/stalwartlabs/smtp-server/discussions),
[Reddit](https://www.reddit.com/r/stalwartlabs) or [Discord](https://discord.gg/9dXkHzCk).
Additionally you may become a sponsor to obtain priority support from Stalwart Labs Ltd.

## Documentation

Table of Contents

- Get Started
  - [Linux / MacOS](/smtp/get-started/linux/)
  - [Windows](/smtp/get-started/windows/)
  - [Docker](/smtp/get-started/docker/)
- Configuration
  - [Overview](/smtp/settings/overview)
  - [Configuration Rules](/smtp/settings/rules)
  - [General settings](/smtp/settings/general)
  - [Remote hosts](/smtp/settings/remote)
  - [Databases](/smtp/settings/database)
  - [Local Lists](/smtp/settings/list)
  - [Tracing & Logging](/smtp/settings/tracing)
- Inbound settings
  - [Listeners](/smtp/inbound/listeners)
  - [Sessions](/smtp/inbound/session)
  - [EHLO Stage](/smtp/inbound/ehlo)
  - [MAIL Stage](/smtp/inbound/mail)
  - [RCPT Stage](/smtp/inbound/rcpt)
  - [DATA Stage](/smtp/inbound/data)
  - [AUTH Stage](/smtp/inbound/auth)
  - [DNSBLs](/smtp/inbound/dnsbl)
  - [Sieve Scripting](/smtp/inbound/sieve)
  - [Throttling](/smtp/inbound/throttle)
- Outbound settings
  - [Queues](/smtp/outbound/queue)
  - [Transport & Routing](/smtp/outbound/transport)
  - [TLS Security](/smtp/outbound/tls)
  - [Throttling](/smtp/outbound/throttle)
  - [Quotas](/smtp/outbound/quota)
  - [DNS](/smtp/outbound/dns)
- Email Authentication
  - [DKIM](/smtp/auth/dkim)
  - [SPF](/smtp/auth/spf)
  - [ARC](/smtp/auth/arc)
  - [DMARC](/smtp/auth/dmarc)
  - [Reverse IP](/smtp/auth/iprev)
  - [Report Analysis](/smtp/auth/analysis)
- Management
  - [API](/smtp/management/api)
  - [CLI](/smtp/management/cli)
  - [Queue](/smtp/management/queue)
  - [Reports](/smtp/management/reports)
- Development
  - [Compiling](/smtp/development/compile/)
  - [Tests](/smtp/development/test/)
  - [RFCs conformed](/smtp/development/rfc/)
