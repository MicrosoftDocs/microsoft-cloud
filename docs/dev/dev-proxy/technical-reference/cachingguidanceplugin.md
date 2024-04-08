---
title: CachingGuidancePlugin
description: CachingGuidancePlugin reference
author: garrytrinder
ms.author: garrytrinder
ms.date: 04/08/2024
---

# CachingGuidancePlugin

Shows a warning when Dev Proxy intercepted the same request within the specified period of time.

:::image type="content" source="../media/guidance-caching.png" alt-text="Screenshot of a command prompt with the Dev Proxy caching guidance plugin showing a warning about a request that's been issued too frequently." lightbox="../media/guidance-caching.png":::

## Plugin instance definition

```json
{
  "name": "CachingGuidancePlugin",
  "enabled": true,
  "pluginPath": "~appFolder/plugins/dev-proxy-plugins.dll",
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
