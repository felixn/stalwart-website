---
title: "Tests"
description: ""
date: 2022-08-28T12:49:20Z
lastmod: 2022-08-28T12:49:20Z
draft: false
images: []
type: docs
menu:
  smtp:
    parent: "development"
    identifier: "test"
weight: 702
toc: true
---

## Overview

The following subsections cover the different tests available on Stalwart SMTP.
Before running these tests, make sure you have Rust installed by following the instructions
in the [compile section](/smtp/development/compile).

## Testing

The base tests perform protocol compliance tests as well as basic functionality testing on 
different functions across the Stalwart SMTP code base. 
To run the base test suite execute:

```bash
cargo test
```

## Fuzzing

To run the fuzz tests please refer to the Stalwart libraries that handle parsing for the SMTP server: [smtp-proto](https://github.com/stalwartlabs/smtp-proto),
[mail-parser](https://github.com/stalwartlabs/mail-parser),
[mail-auth](https://github.com/stalwartlabs/mail-auth) and [sieve-rs](https://github.com/stalwartlabs/sieve). 

