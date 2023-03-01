---
title: "API"
description: ""
date: 2022-08-28T12:49:20Z
lastmod: 2022-08-28T12:49:20Z
draft: false
images: []
type: docs
menu:
  smtp:
    parent: "manage"
    identifier: "api"
weight: 601
toc: true
---

## Overview

Stalwart SMTP features an HTTP-based management API that provides system administrators with the ability to manage message queues and scheduled DMARC and TLS aggregate reports. The API can be utilized through the [Command Line Interface](/smtp/management/cli), using curl, or even automated through scripts.

## Settings

To enable the management interface in Stalwart SMTP, a special type of [listener](/smtp/inbound/listeners) that uses the HTTP protocol has to be created. This can be done by specifying the IP address(es) and port(s) for the management API to listen for incoming connections in the `server.listener.<id>.bind` attribute and setting the `server.listener.<id>.protocol` attribute to `http`.

For instance, to enable the HTTP management API on 127.0.0.1 port 8686:

```txt
[server.listener."management"]
bind = ["127.0.0.1:8686"]
protocol = "http"
```

The management interface will automatically enable the default TLS certificate used by the SMTP listener(s), but this can be [overridden](/smtp/inbound/listeners) from the configuration file.

## Authentication

Management requests must be authenticated and the list of authorized username and passwords can be configured through the `management.lookup.auth` lookup. For example, to use a local authentication method that authorizes accounts `tim` with password `abcde` and `tom` with password `12345`:

```txt
[management.lookup]
auth = ["list/admin"]

[list]
admin = ["tim:abcde", "tom:12345"]
```

Other authentication mechanisms such as SQL [databases](/smtp/settings/database) can be used for authorizing management requests too.
