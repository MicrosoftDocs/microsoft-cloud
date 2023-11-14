---
title: GraphBetaSupportGuidancePlugin
description: GraphBetaSupportGuidancePlugin reference
author: garrytrinder
ms.author: garrytrinder
ms.contributors: garrytrinder
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

# GraphBetaSupportGuidancePlugin

Provides guidance that recommends not using beta APIs in production when a request is made to Microsoft Graph beta endpoint.

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
