---
title: CachingGuidancePlugin
description: CachingGuidancePlugin reference
author: garrytrinder
ms.author: garrytrinder
ms.date: 01/06/2026
---

<!-- INTENT: Warn about repeated requests that could be cached -->
<!-- PLUGIN-TYPE: Intercepting -->
<!-- WORKS-WITH: LatencyPlugin, ExecutionSummaryPlugin -->
<!-- USE-WHEN: Optimizing API call patterns and identifying caching opportunities -->

# CachingGuidancePlugin

Shows a warning when Dev Proxy intercepted the same request within the specified period of time.

:::image type="content" source="../media/guidance-caching.png" alt-text="Screenshot of a command prompt with the Dev Proxy caching guidance plugin showing a warning about a request that's been issued too frequently." lightbox="../media/guidance-caching.png":::

## Configuration example

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/rc.schema.json",
  "plugins": [
    {
      "name": "CachingGuidancePlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "cachingGuidance"
    }
  ],
  "cachingGuidance": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/cachingguidanceplugin.schema.json",
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
