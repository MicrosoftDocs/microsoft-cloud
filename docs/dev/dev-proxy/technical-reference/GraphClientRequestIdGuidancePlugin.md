---
title: GraphClientRequestIdGuidancePlugin
description: GraphClientRequestIdGuidancePlugin reference
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

# GraphClientRequestIdGuidancePlugin

Shows a tip when a request to Microsoft Graph API doesn't include the `client-request-id` header.

:::image type="content" source="../media/microsoft-graph-client-request-id-guidance.png" alt-text="Screenshot of a terminal with Dev Proxy showing a tip after detecting a request to Microsoft Graph API without the client-request-id header." lightbox="../media/microsoft-graph-client-request-id-guidance.png":::

## Plugin instance definition

```json
{
  "name": "GraphClientRequestIdGuidancePlugin",
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

## Command line options

None
