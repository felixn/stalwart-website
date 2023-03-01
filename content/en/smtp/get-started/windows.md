---
title: "Windows"
description: ""
lead: ""
date: 2022-01-25T14:41:39+01:00
lastmod: 2022-01-25T14:41:39+01:00
draft: false
images: []
type: docs
menu:
  smtp:
    parent: "get-started"
    identifier: "windows"
weight: 102
toc: true
---

## Installing

Unfortunately an automated installer is not yet available for Windows systems. To perform
a manual installation in Windows, follow these steps:

- Download the template [configuration](https://raw.githubusercontent.com/stalwartlabs/smtp-server/main/resources/config/stalwart-config.zip) and uncompress it as ``<BASE_PATH>``.
- Edit ``<BASE_PATH>/etc/config.toml`` and replace all occurrences of the following strings:
    - ``__HOST__`` with your SMTP server's fully-qualified domain name, for example "mx.mydomain.org".
    - ``__DOMAIN__`` with your main domain name, for example "mydomain.org".
    - ``__ADMIN_PASS__`` with the administrator password to use to manage Stalwart SMTP.
    - ``__LMTP_HOST__`` and ``__LMTP_HOST__`` with the address and port to use to deliver messages to your message store (e.g.: Stalwart JMAP, Dovecot, Courier-IMAP, etc.) over the LMTP protocol.

*Note*: At the moment only x86 binaries are distributed but an ARM64 Windows version can be compiled from the sources.

## Setting up

Before your Stalwart SMTP server can start accepting connections from other hosts, you'll have to configure your hostname and install valid a TLS certificate.

### TLS

Stalwart SMTP requires a valid TLS certificate in order to operate. If you currently don't have a TLS certificate, 
you can obtain one for free from [Let's Encrypt](https://letsencrypt.org/). 
Once you have your certificate ready, copy your certificate and private key to their default locations as follows:

```bash
copy mycert.crt C:\Program Files\Stalwart SMTP\etc\certs\tls.crt
copy mykey.key C:\Program Files\Stalwart SMTP\etc\private\tls.key
```

### DKIM

DomainKeys Identified Mail (DKIM) is a method of email authentication that allows a receiving email server to verify that an email message was actually sent by the owner of the domain from which it appears to have been sent. It is highly recommended that you enable DKIM (as well as SPF and DMARC) for your domain.
Please refer to the [DKIM](/smtp/auth/dkim) section for instructions on how to add a new DKIM signature.

## Start service

To run Stalwart SMTP as a service, follow these instructions:

- Download the [NSSM](http://nssm.cc/download) service manager.
- Run in your terminal:
  ```
  nssm install Stalwart_SMTP
  ```
- Once the NSSM GUI appears, configure the service using the following parameters:
  ```
  Path: C:\Program Files\Stalwart SMTP\bin\stalwart-jmap.exe
  Startup directory: C:\Program Files\Stalwart SMTP
  Arguments: --config=C:\Program Files\Stalwart SMTP\etc\config.toml
  ```
- Click on the Install Service button.

If everything is correct, you should now be able to connect to Stalwart SMTP on ports ``25``, ``587`` and ``465``

## Next steps

Now that you have Stalwart SMTP up and running, you may want to enable [SASL authentication](/smtp/inbound/auth) using
your IMAP4 server or SQL database.

