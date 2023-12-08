---
title: GraphBetaSupportGuidancePlugin
description: GraphBetaSupportGuidancePlugin reference
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

# GraphBetaSupportGuidancePlugin

Shows a warning when proxy detects a request to Microsoft Graph beta endpoint.

:::image type="content" source="../media/microsoft-graph-beta-guidance.png" alt-text="Screenshot of a terminal with Dev Proxy showing a warning after detecting a request to a Microsoft Graph beta endpoint." lightbox="../media/microsoft-graph-beta-guidance.png":::

## Plugin instance definition

```json
{
  "name": "GraphBetaSupportGuidancePlugin",
  "enabled": true,
  "pluginPath": "plugins\\dev-proxy-plugins.dll",
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
