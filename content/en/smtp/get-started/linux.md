---
title: "Linux / MacOS"
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
    identifier: "linux"
weight: 101
toc: true
---

## Install

Install Stalwart SMTP server by running the following command in your terminal:

```bash
curl --proto '=https' --tlsv1.2 -sSf https://smtp.stalw.art/install.sh | sudo sh
```

Once the installation is completed, Stalwart SMTP will be available under the ``/usr/local/stalwart-smtp``
directory. The installation script will also create the ``stalwart-smtp`` account and start the ``stalwart-smtp`` systemd/launchd service.

Please note that _root access_ is required to perform the installation, if you don't feel comfortable running the install script as root
you may also [download the latest release](https://github.com/stalwartlabs/smtp-server/releases) and
perform a manual installation.

## Set up

The installating script will ask you to enter your server's fully-qualified domain-name (FQDN), your domain name (you can add more domains later)
and the administrator's password (used by the management REST API). Additionally you'll have to provide the address and port to use to deliver 
messages to your message store (e.g.: Stalwart JMAP, Dovecot, Courier-IMAP, etc.) over the LMTP protocol:

```txt
Enter the SMTP server's hostname [mx.yourdomain.org]: mx.mydomain.org
Enter your domain name [yourdomain.org]: mydomain.org
Enter the SMTP server's admininstrator password [changeme]: my_secret_pass
Enter your LMTP server's hostame [localhost]: localhost
Enter your LMTP server's port [24]: 24
```

If everything is correct, you should now be able to connect to Stalwart SMTP on ports ``25``, ``587`` and ``465`` (using a self-signed certificate), but
before it can start accepting connections from SMTP clients, you'll have to install valid a TLS certificate.

### TLS

Stalwart SMTP requires a valid TLS certificate in order to operate. If you currently don't have a TLS certificate, 
you can obtain one for free from [Let's Encrypt](https://letsencrypt.org/). 
Once you have your certificate ready, copy your certificate and private key to their default locations as follows:

```bash
sudo cp mycert.crt /usr/local/stalwart-smtp/etc/certs/tls.crt
sudo cp mykey.key /usr/local/stalwart-smtp/etc/private/tls.key
```

### DKIM

DomainKeys Identified Mail (DKIM) is a method of email authentication that allows a receiving email server to verify that an email message was actually sent by the owner of the domain from which it appears to have been sent. It is highly recommended that you enable DKIM (as well as SPF and DMARC) for your domain.
The installation script will automatically generate a 2048 bits RSA certificate for your domain and print out the instructions to enable DKIM in your DNS server:

```txt
To enable DKIM please add the following record to your DNS server:

Record: stalwart_smtp._domainkey.mydomain.org
Value: v=DKIM1; p=MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAux8eFQWzgUCHZ105H+D+oDI/tzovKRYInWIAq2pH1H1Osr5jKMhbN2fKW3f9gk810cC50aR4oo2DRKfwUqDlLJmum6UWfcliSwTwIDloGnUTWNeSw3U/ZvsmMAg00rwWA2OxPj59se7ElwfDICEPlxuePxI8l+TaAbq1fN4OJPJOC9ERn11JslR5D/wLIzKX5e6mM4MnNfo2dC0y7JKtn6w0gnNegERN3gmdPLnxccMhVBHYkQz2rPYOki5jnGkRuhcY0F1X+wVgRbiuaBv4X5VbebheyOjsv/bxRwAWVYFwCQJP1FoG7lQbr3YtCQihR8RzVclvSL8Y68uxL+qHwQIDAQAB
Type: TXT
TTL: 86400
```

If you already have a DKIM certificate simply ignore these instructions and refer to the [DKIM](/auth/dkim) section for instructions on how to add a new DKIM signature.

## Restart service

Once you have completed the setup instructions, restart your Stalwart SMTP server:

```bash
sudo systemctl restart stalwart-smtp
```

Or, if you are using MacOS:

```bash
sudo launchctl kickstart -k stalwart.smtp
```

## Next steps

Now that you have Stalwart SMTP up and running, you may want to enable [SASL authentication]() using
your IMAP4 server or SQL database.

