---
title: "Docker"
description: ""
date: 2022-08-28T12:49:20Z
lastmod: 2022-08-28T12:49:20Z
draft: false
images: []
type: docs
menu:
  smtp:
    parent: "get-started"
    identifier: "docker"
weight: 103
toc: true
---

## Pre-Install

Before you can run the Stalwart SMTP Docker container, you will have to create the configuration file and default directories on your Docker host: 

- Download the template [configuration](https://raw.githubusercontent.com/stalwartlabs/smtp-server/main/resources/config/stalwart-config.zip) and uncompress it as ``<BASE_PATH>``.
- Edit ``<BASE_PATH>/etc/config.toml`` and replace all occurrences of the following strings:
    - ``__HOST__`` with your SMTP server's fully-qualified domain name, for example "mx.mydomain.org".
    - ``__DOMAIN__`` with your main domain name, for example "mydomain.org".
    - ``__ADMIN_PASS__`` with the administrator password to use to manage Stalwart SMTP.
    - ``__LMTP_HOST__`` and ``__LMTP_PORT__`` with the address and port to use to deliver messages to your message store (e.g.: Stalwart JMAP, Dovecot, Courier-IMAP, etc.) over the LMTP protocol.

### TLS

Stalwart SMTP requires a valid TLS certificate in order to operate. If you currently don't have a TLS certificate, 
you can obtain one for free from [Let's Encrypt](https://letsencrypt.org/). 
Once you have your certificate ready, copy your certificate and private key to their default locations as follows:

```bash
sudo cp mycert.crt <BASE_PATH>/etc/certs/tls.crt
sudo cp mykey.key <BASE_PATH>/etc/private/tls.key
```

### DKIM

DomainKeys Identified Mail (DKIM) is a method of email authentication that allows a receiving email server to verify that an email message was actually sent by the owner of the domain from which it appears to have been sent. It is highly recommended that you enable DKIM (as well as SPF and DMARC) for your domain.
Please refer to the [DKIM](/smtp/auth/dkim) section for instructions on how to add a new DKIM signature.

## Install

Once you have completed the above steps, execute in your terminal:

```bash
docker run -d -ti -p 25:25 -p 465:495 -p 587:587 -p 8686:8686 \
           -v <BASE_PATH>:/usr/local/stalwart-smtp \
           --name stalwart-smtp stalwartlabs/smtp-server:latest \
           --config /usr/local/stalwart-smtp/etc/config.toml
```

Replace ``<BASE_PATH>`` with the directory on the Docker host where the Stalwart SMTP data will reside.

If everything is correct, you should now be able to connect to Stalwart SMTP on ports ``25``, ``587`` and ``465``

## Install CLI

In order to manage your Stalwart SMTP instance, you are also going to need to install the Stalwart CLI (command line interface).
The CLI tool can be installed by following these instructions on the same server where Stalwart SMTP 
is running or any other computer with internet access to your server:

- **Linux / MacOS**: 

    ```bash
    curl --proto '=https' --tlsv1.2 -sSf https://smtp-cli.stalw.art/install.sh | sh
    ```
    Once the installation is completed, the CLI tool will be available in your home directory at ``$HOME/.stalwart/stalwart-cli``. You may add the
    ``$HOME/.stalwart`` directory to your ``PATH`` environment variable or move the ``stalwart-cli`` binary to a location that is already
    on your ``PATH`` variable.

- **Windows**: 
  
    Download the CLI tool directly from [here](https://github.com/stalwartlabs/cli/releases/latest/download/stalwart-cli-x86_64-pc-windows-msvc.zip).

## Next steps

Now that you have Stalwart SMTP up and running, you may want to enable [SASL authentication](/smtp/inbound/auth) using
your IMAP4 server or SQL database.

