---
title: GraphBetaSupportGuidancePlugin
description: GraphBetaSupportGuidancePlugin reference
author: garrytrinder
ms.author: garrytrinder
ms.date: 04/08/2024
---

# GraphBetaSupportGuidancePlugin

Shows a warning when proxy detects a request to Microsoft Graph beta endpoint.

:::image type="content" source="../media/microsoft-graph-beta-guidance.png" alt-text="Screenshot of a command prompt with Dev Proxy showing a warning after detecting a request to a Microsoft Graph beta endpoint." lightbox="../media/microsoft-graph-beta-guidance.png":::

## Plugin instance definition

```json
{
  "name": "GraphBetaSupportGuidancePlugin",
  "enabled": true,
  "pluginPath": "~appFolder/plugins/dev-proxy-plugins.dll",
  "urlsToWatch": [
    "https://graph.microsoft.com/beta/*",
    "https://graph.microsoft.us/beta/*",
    "https://dod-graph.microsoft.us/beta/*",
    "https://microsoftgraph.chinacloudapi.cn/beta/*"
  ]
}
```

## Configuration example

None

## Configuration properties

None

## Command line options

None
