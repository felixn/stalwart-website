---
title: "General settings"
description: ""
date: 2022-08-28T12:49:20Z
lastmod: 2022-08-28T12:49:20Z
draft: false
images: []
type: docs
menu:
  smtp:
    parent: "settings"
    identifier: "general"
weight: 203
toc: true
---

## Overview

This section of the configuration manual deals with some of the general settings of Stalwart SMTP.

## Server Hostname

The server hostname is utilized in SMTP `EHLO` commands, as well as included in message headers and reports.
This setting is configured using the server.hostname parameter in the configuration file:

```txt
[server]
hostname = "mx.mydomain.org"
```

## Run as user

On Unix/Linux systems, Stalwart SMTP requires the `root` user's privileges to bind to privileged ports. Afterward, these privileges are dropped, and Stalwart SMTP operates using the UID/GID of a non-privileged account. The non-privileged account's UID is configured with the `server.run-as.user` attribute, while the GID is configured with the `server.run-as.group` attribute. For example:

```txt
[server.run-as]
user = "stalwart-smtp"
group = "stalwart-smtp"
```

## Default concurrency

The default concurrency defines the maximum number of simultaneous connections that Stalwart SMTP can handle at any given moment. This setting is controlled by the `global.concurrency` parameter in the configuration file:

```txt
[global]
concurrency = 1024
```

## Thread pool size

Stalwart SMTP utilizes a thread pool for the execution of CPU-intensive tasks. The default size of the thread pool is equal to the number of CPUs available on the server. However, the size of the thread pool can be manually adjusted using the `global.thread-pool` parameter in the configuration file:

```txt
[global]
thread-pool = 16
```

It is important to keep in mind that setting the size of the thread pool to a value higher than the number of CPUs available on the server does not guarantee improved performance.

## Shared map

The shared map is an in-memory data structure that is accessible to multiple threads and serves as the storage location for the various rate, concurrency, and quota limiters. The default number of shards for the shared map is controlled by the `global.shared-map.shard` parameter, while the default capacity of each shard can be adjusted using the `global.shared-map.capacity` parameter in the configuration file:

```txt
[global]
shared-map = {shard = 32, capacity = 10}
```

