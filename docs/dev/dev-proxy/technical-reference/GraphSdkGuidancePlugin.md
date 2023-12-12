---
title: GraphSdkGuidancePlugin
description: GraphSdkGuidancePlugin reference
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

# GraphSdkGuidancePlugin

Shows a tip when proxy intercepts a request to Microsoft Graph that hasn't been issued by a Microsoft Graph SDK.

:::image type="content" source="../media/microsoft-graph-sdk-guidance.png" alt-text="Dev Proxy showing a tip suggesting using the Microsoft Graph SDK after intercepting a request to Microsoft Graph that doesn't use an SDK" lightbox="../media/microsoft-graph-sdk-guidance.png":::

## Plugin instance definition

```json
{
  "name": "GraphSdkGuidancePlugin",
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
