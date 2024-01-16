---
title: ODataPagingGuidancePlugin
description: ODataPagingGuidancePlugin reference
author: garrytrinder
ms.author: garrytrinder
ms.date: 01/16/2024
---

# ODataPagingGuidancePlugin

Shows a warning when proxy intercepts an OData paging request using a URL that hasn't been previously returned in one of the intercepted responses.

:::image type="content" source="../media/odata-paging-guidance.png" alt-text="Screenshot of a terminal with Dev Proxy showing warning after intercepting a request with manually crafted skip token." lightbox="../media/odata-paging-guidance.png":::

## Plugin instance definition

```json
{
  "name": "ODataPagingGuidancePlugin",
  "enabled": true,
  "pluginPath": "~appFolder/plugins/dev-proxy-plugins.dll"
}
```

## Configuration example

None

## Configuration properties

None

## Command line options

None
