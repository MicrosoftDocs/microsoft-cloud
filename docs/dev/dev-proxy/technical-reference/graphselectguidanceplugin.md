---
title: GraphSelectGuidancePlugin
description: GraphSelectGuidancePlugin reference
author: garrytrinder
ms.author: garrytrinder
ms.date: 01/06/2026
---

<!-- INTENT: Warn about missing $select in Microsoft Graph calls -->
<!-- PLUGIN-TYPE: Intercepting -->
<!-- WORKS-WITH: GraphBetaSupportGuidancePlugin, GraphSdkGuidancePlugin, CachingGuidancePlugin -->
<!-- USE-WHEN: Optimizing Microsoft Graph API performance -->

# GraphSelectGuidancePlugin

Shows a warning when proxy intercepts a request to Microsoft Graph APIs that doesn't include the `$select` query string parameter.

:::image type="content" source="../media/microsoft-graph-select-guidance.png" alt-text="Screenshot of a command prompt with Dev Proxy showing a warning after intercepting a request to Microsoft Graph that doesn't use the $select parameter." lightbox="../media/microsoft-graph-select-guidance.png":::

## Configuration example

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/rc.schema.json",
  "plugins": [
    {
      "name": "GraphSelectGuidancePlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll"
    }
  ],
  "urlsToWatch": [
    "https://graph.microsoft.com/v1.0/*",
    "https://graph.microsoft.com/beta/*",
    "https://graph.microsoft.us/v1.0/*",
    "https://graph.microsoft.us/beta/*",
    "https://dod-graph.microsoft.us/v1.0/*",
    "https://dod-graph.microsoft.us/beta/*",
    "https://microsoftgraph.chinacloudapi.cn/v1.0/*",
    "https://microsoftgraph.chinacloudapi.cn/beta/*"
  ]
}
```

## Configuration properties

None
