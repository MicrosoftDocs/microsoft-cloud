---
title: GraphBetaSupportGuidancePlugin
description: GraphBetaSupportGuidancePlugin reference
author: garrytrinder
ms.author: garrytrinder
ms.date: 01/06/2026
---

<!-- INTENT: Warn about Microsoft Graph beta endpoint usage -->
<!-- PLUGIN-TYPE: Intercepting -->
<!-- WORKS-WITH: GraphSelectGuidancePlugin, GraphSdkGuidancePlugin -->
<!-- USE-WHEN: Ensuring production readiness by avoiding beta APIs -->

# GraphBetaSupportGuidancePlugin

Shows a warning when proxy detects a request to Microsoft Graph beta endpoint.

:::image type="content" source="../media/microsoft-graph-beta-guidance.png" alt-text="Screenshot of a command prompt with Dev Proxy showing a warning after detecting a request to a Microsoft Graph beta endpoint." lightbox="../media/microsoft-graph-beta-guidance.png":::

## Configuration example

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.0.0/rc.schema.json",
  "plugins": [
    {
      "name": "GraphBetaSupportGuidancePlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll"
    }
  ],
  "urlsToWatch": [
    "https://graph.microsoft.com/beta/*",
    "https://graph.microsoft.us/beta/*",
    "https://dod-graph.microsoft.us/beta/*",
    "https://microsoftgraph.chinacloudapi.cn/beta/*"
  ]
}
```

## Configuration properties

None

## Command line options

None
