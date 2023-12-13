---
title: GraphSelectGuidancePlugin
description: GraphSelectGuidancePlugin reference
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
---

# GraphSelectGuidancePlugin

Shows a warning when proxy intercepts a request to Microsoft Graph APIs that doesn't include the `$select` query string parameter.

:::image type="content" source="../media/microsoft-graph-select-guidance.png" alt-text="Screenshot of a terminal with Dev Proxy showing a warning after intercepting a request to Microsoft Graph that doesn't use the $select parameter" lightbox="../media/microsoft-graph-select-guidance.png":::

## Plugin instance definition

```json
{
  "name": "GraphSelectGuidancePlugin",
  "enabled": true,
  "pluginPath": "plugins\\dev-proxy-plugins.dll",
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

## Configuration example

None

## Configuration properties

None
