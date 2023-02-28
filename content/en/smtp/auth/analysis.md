---
title: "Report Analysis"
description: ""
date: 2022-08-28T12:49:20Z
lastmod: 2022-08-28T12:49:20Z
draft: false
images: []
type: docs
menu:
  smtp:
    parent: "auth"
    identifier: "analysis"
weight: 506
toc: true
---

## Overview

Stalwart SMTP has the ability to automatically analyze incoming DMARC, DKIM, SPF, and TLS reports that are sent by other domains, which eliminates the need for manual intervention and saves time and effort for the system administrator. In case any TLS or message authentication issues are found, an event is recorded in the log file or sent to [OpenTelemetry](/smtp/settings/tracing). By turning reports into actionable events, system administrators can quickly detect and respond to configuration errors and any instances of abuse, such as spam or phishing, which helps to maintain the integrity of the email system.

## Settings

The configuration for report analysis can be found under the `report.analysis` key and includes the following attributes:

- `addresses`: A list of addresses (which may include wildcards) from which reports will be intercepted and analyzed.
- `forward`: Whether reports should be forwarded to their final recipient after analysis.
- `store`: The location where incoming reports will be stored. If no path is specified, the reports will not be stored.

Example:

```txt
[report.analysis]
addresses = ["dmarc@*", "abuse@*"]
forward = true
store = "/usr/local/stalwart-smtp/incoming"
```
