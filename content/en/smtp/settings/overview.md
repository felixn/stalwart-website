---
title: "Overview"
description: ""
date: 2022-08-28T12:49:20Z
lastmod: 2022-08-28T12:49:20Z
draft: false
images: []
type: docs
menu:
  smtp:
    parent: "settings"
    identifier: "overview"
weight: 201
toc: true
---

## Introduction

Stalwart SMTP uses the [TOML](https://toml.io/en/) format for its configuration file, which is usually located under `<BASE_PATH>/etc/config.toml`.
TOML, which stands for Tom's Obvious, Minimal Language, is a simple and easy-to-read configuration file format that is designed to be minimal and easy to understand. It is well-suited for storing configuration information as it provides a clear and concise way to specify the various settings required to configure Stalwart SMTP server.

## Value types

In the Stalwart SMTP configuration file, settings may expect values in the form of strings, integers, sizes, booleans, IP addresses, durations, rates, lookup paths, or arrays that contain any of these types.

### Strings

Strings can be represented in a few different ways:

- Basic Strings: A basic string is represented by a sequence of characters enclosed in double quotes `"`. For example, `"Hello, world!"`.
- Multiline Strings: Multiline strings are represented by a sequence of characters enclosed in triple quotes `"""`. This type of string can span multiple lines and preserve line breaks within the string. For example,

```txt
multiline_string = """
This is a multiline
string in TOML format.
"""
```
- Literal Strings: Literal strings are represented by a sequence of characters enclosed in single quotes `'`. This type of string is useful for representing strings that contain characters that would normally need to be escaped within a basic or multiline string, such as double quotes. For example, `'Hello, "world"'`.

In a TOML file, strings can be used as values for keys or as elements in arrays. No matter how a string is represented in a TOML file, it must be enclosed in quotes or string literals in order to be treated as a string.

### Integers

Integers can be represented as a negative or positive whole number without a decimal point, for example:

```txt
positive_integer = 42
negative_integer = -42
```

It's important to note that in TOML, integers cannot contain leading zeros or a plus sign before the number. Integers can be used as values for keys or as elements in arrays. 

### Booleans

Boolean values can be represented as either true or false without quotes, for example:

```txt
is_enabled = true
is_disabled = false
```

In a TOML file, booleans can be used as values for keys or as elements in arrays. 

### Sizes

Sizes are represented in bytes as positive integers without a decimal point, for example:

```txt
size = 1024
```

### IP addresses

IP addresses are represented as strings and can contain either IPv4 or IPv6 addresses, for example:

```txt
ip_v4 = "192.168.0.1"
ip_v6 = "ab:3::f:9"
```

### Durations

Durations are represented as a string that consists of an integer value and a unit of measurement. The following units of measurement are available for representing durations:

- `ms`: Milliseconds
- `s`: Seconds
- `m`: Minutes
- `h`: Hours
- `d`: Days 

For example:

```txt
timeout = "30s"
```

In this example, the duration `30s` represents 30 seconds. The integer value and the unit are separated by the unit symbol, and the entire string must be enclosed in quotes or string literals in order to be treated as a string in the TOML file.

### Rates

Rates are represented as strings that consist of an integer value followed by the symbol `/` and a duration. The duration specifies the time interval over which the integer value is to be distributed. For example:

```txt
rate1 = "20 / 5m"  # 20 every 5 minutes
rate2 = "1 / 1d"  # 1 per day
```

In the first example, `20 / 5m` represents a rate of 20 occurrences per 5 minutes. In the second example, `1 / 1d` represents a rate of 1 occurrence per day. The integer value, the `/` symbol, and the duration must be enclosed in quotes or string literals in order to be treated as a string in the TOML file.

### Lookup paths

In the configuration file, certain attributes expect lookup paths as values. These lookup paths are formatted as `<lookup_type>/<lookup_id>[/<lookup_table>]` and refer to local lists, database queries, or remote SMTP, LMTP, or IMAP servers defined within the configuration. The types of lookups that can be used are:

- `list`: Refers to a local [list](/smtp/settings/list) containing values.
- `db`: Refers to a query in a [database](/smtp/settings/database).
- `remote`: Refers to a [remote](/smtp/settings/remote) SMTP, LMTP, or IMAP server.

Some examples of lookup paths are:

```txt
auth = "db/production_db/paid_clients"
addresses = "remote/lmtp"
domains = "list/local_domains"
```

### Arrays

Arrays are sequences of values that can be of any type, including strings, integers, booleans or any other type. Arrays are represented using square brackets `[]` and are separated by commas `,`. For example:

```txt
array_of_strings = [ "apple", "banana", "cherry" ]
array_of_integers = [ 1, 2, 3 ]
```
