---
title: ODataPagingGuidancePlugin
description: ODataPagingGuidancePlugin reference
author: garrytrinder
ms.author: garrytrinder
ms.date: 01/06/2026
---

<!-- INTENT: Warn about incorrect OData paging patterns -->
<!-- PLUGIN-TYPE: Intercepting -->
<!-- WORKS-WITH: GraphSelectGuidancePlugin, CachingGuidancePlugin -->
<!-- USE-WHEN: Ensuring correct OData pagination implementation -->

# ODataPagingGuidancePlugin

Shows a warning when proxy intercepts an OData paging request using a URL that hasn't been previously returned in one of the intercepted responses.

:::image type="content" source="../media/odata-paging-guidance.png" alt-text="Screenshot of a command prompt with Dev Proxy showing warning after intercepting a request with manually crafted skip token." lightbox="../media/odata-paging-guidance.png":::

## Configuration example

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.0.0/rc.schema.json",
  "plugins": [
    {
      "name": "ODataPagingGuidancePlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll"
    }
  ]
}
```

## Configuration properties

None

## Command line options

None
