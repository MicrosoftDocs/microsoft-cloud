---
title: GraphMockResponsePlugin
description: GraphMockResponsePlugin reference
author: garrytrinder
ms.author: garrytrinder
ms.date: 01/06/2026
---

<!-- INTENT: Mock responses for Microsoft Graph API including batch requests -->
<!-- PLUGIN-TYPE: Intercepting -->
<!-- WORKS-WITH: MockGeneratorPlugin, GraphMinimalPermissionsPlugin, LatencyPlugin -->
<!-- USE-WHEN: Mocking Microsoft Graph APIs with batch support -->

# GraphMockResponsePlugin

Mocks responses to Microsoft Graph APIs. Includes all functionality of the [MockResponsePlugin](./MockResponsePlugin.md) and adds support for mocking Microsoft Graph API batch requests.

:::image type="content" source="../media/microsoft-graph-mock-response.png" alt-text="Screenshot of a command prompt with Dev Proxy mocking a response to a Microsoft Graph API." lightbox="../media/microsoft-graph-mock-response.png":::

## Configuration example

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.0.0/rc.schema.json",
  "plugins": [
    {
      "name": "GraphMockResponsePlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "mocksPlugin"
    }
  ],
  "mocksPlugin": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.0.0/mockresponseplugin.schema.json",
    "mocksFile": "mocks.json"
  }
}
```

See [MockResponsePlugin](./MockResponsePlugin.md) for more configuration options.

## Configuration properties

See [MockResponsePlugin](./MockResponsePlugin.md)

## Command line options

See [MockResponsePlugin](./MockResponsePlugin.md)
