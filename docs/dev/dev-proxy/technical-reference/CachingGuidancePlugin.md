---
title: CachingGuidancePlugin
description: CachingGuidancePlugin reference
author: garrytrinder
ms.author: garrytrinder
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
  - tool=devproxy
---

# CachingGuidancePlugin

Shows a warning when Dev Proxy intercepted the same request within the specified period of time.

:::image type="content" source="../media/guidance-caching.png" alt-text="Screenshot of a terminal with the Dev Proxy caching guidance plugin showing a warning about a request that's been issued too frequently." lightbox="../media/guidance-caching.png":::

## Plugin instance definition

```json
{
  "name": "CachingGuidancePlugin",
  "enabled": true,
  "pluginPath": "plugins\\dev-proxy-plugins.dll",
  "configSection": "cachingGuidance"
}
```

## Configuration example

```json
{
  "cachingGuidance": {
    "cacheThresholdSeconds": 5
  }
}
```

## Configuration properties

| Property | Description | Default |
|----------|-------------|--------:|
| `cacheThresholdSeconds` | The number of seconds between the same request that triggers the guidance warning. | 5 |

## Command line options

None
