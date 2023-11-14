---
title: RetryAfterPlugin
description: RetryAfterPlugin reference
author: garrytrinder
ms.author: garrytrinder
ms.contributors: garrytrinder
ms.date: 11/03/2023
ms.topic: conceptual
ms.service: microsoft-cloud-for-developers

categories:
  - developer-tools
products:
  - microsoft-365
  - microsoft-graph
  - sharepoint-online
  - m365
ms.custom:
  - fcp
  - team=cloud_advocates
---

# RetryAfterPlugin

Simulates the `Retry-After` header sent by an API after throttling a request. Also, shows a warning when proxy detects a subsequent request to the same URL before the `Retry-After` time has elapsed.

## Plugin instance definition

```json
{
  "name": "RetryAfterPlugin",
  "enabled": true,
  "pluginPath": "plugins\\dev-proxy-plugins.dll"
}
```

## Configuration example

None

## Configuration properties

None

## Command line options

None
