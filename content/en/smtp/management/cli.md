---
title: "CLI"
description: ""
date: 2022-08-28T12:49:20Z
lastmod: 2022-08-28T12:49:20Z
draft: false
images: []
type: docs
menu:
  smtp:
    parent: "manage"
    identifier: "overview"
weight: 602
toc: true
---

## Overview

The Stalwart Command Line Interface (CLI) allows system administrators to perform tasks
such as managing queues or pending DMARC and TLS reports. Future versions of Stalwart SMTP
will include a web-based management interface, but for the time being the CLI is the main
tool to manage Stalwart SMTP.

## Installation

The CLI tool should be already installed on the server where Stalwart SMTP is running at 
``/usr/local/stalwart-smtp/bin/stalwart-cli``. 
If you would like to install the CLI on a different computer, follow the instructions for 
your platform below:

- **Linux / MacOS**: 

    ```bash
    curl --proto '=https' --tlsv1.2 -sSf https://cli.stalw.art/install.sh | sh
    ```
    Once the installation is completed, the CLI tool will be available in your home directory at ``$HOME/.stalwart/stalwart-cli``. You may add the
    ``$HOME/.stalwart`` directory to your ``PATH`` environment variable or move the ``stalwart-cli`` binary to a location that is already
    on your ``PATH`` variable.

- **Windows**: 
  
    Download the CLI tool directly from [here](https://github.com/stalwartlabs/cli/releases/latest/download/stalwart-cli-x86_64-pc-windows-msvc.zip).

## Usage

The default location of the Stalwart CLI is ``/usr/local/stalwart-smtp/bin/stalwart-cli`` (or ``$HOME/.stalwart/stalwart-cli``
when installed manually). When executed without any parameters, the CLI tool prints a brief help page such as this one:

```bash
$ stalwart-cli

Stalwart Mail Server CLI

Usage: stalwart-cli [OPTIONS] --url <URL> <COMMAND>

Commands:
  account  Manage user accounts
  domain   Manage domains
  list     Manage mailing lists
  group    Manage groups
  import   Import accounts and domains
  queue    Manage SMTP message queue
  report   Manage SMTP DMARC/TLS report queue
  help     Print this message or the help of the given subcommand(s)

Options:
  -u, --url <URL>                  JMAP or SMTP server base URL
  -c, --credentials <CREDENTIALS>  Authentication credentials
  -h, --help                       Print help
  -V, --version                    Print version
```

The CLI tool expects two required arguments: the URL of your Stalwart SMTP management interface (which is 
specified with the ``-u`` option) server and the authentication credentials (which 
may be specified with the ``-c`` option or at the prompt).

For example, to list all messages queued for delivery:

```bash
$ stalwart-cli -u https://mx.example.org -c PASSWORD queue list
```

## Authentication

Stalwart SMTP requires that requests for management tasks to be authenticated. The authentication credentials can be provided either through the command line using the `-c` argument or through a prompt that will appear if the argument is not provided. The credentials should be entered in the format of `account_name:password`. If the account name is not specified, the default system administrator account, admin, will be used. 

For example, to authenticate with ``postmaster`` and password ``secret_pass``:

```bash
$ stalwart-cli -u https://mx.example.org -c postmaster:secret_pass queue list
```

However, using the ``-c`` argument to provide the authentication credentials should be avoided as these
will be recorded in the terminal's history. Instead, type the password at the prompt:

```bash
$ stalwart-cli -u https://mx.example.org queue list

Enter SMTP admin credentials:  ******
```
