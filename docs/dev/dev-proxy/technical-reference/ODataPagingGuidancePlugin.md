---
title: ODataPagingGuidancePlugin
description: ODataPagingGuidancePlugin reference
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

# ODataPagingGuidancePlugin

Shows a warning when proxy intercepts an OData paging request using a URL that hasn't been previously returned in one of the intercepted responses.

:::image type="content" source="../media/odata-paging-guidance.png" alt-text="Dev Proxy showing warning after intercepting a request with manually crafted skip token" lightbox="../media/odata-paging-guidance.png":::

## Plugin instance definition

```json
{
  "name": "ODataPagingGuidancePlugin",
  "enabled": true,
  "pluginPath": "plugins\\dev-proxy-plugins.dll"
}
```

## Configuration example

None

## Configuration properties

None

## Command line options

None
