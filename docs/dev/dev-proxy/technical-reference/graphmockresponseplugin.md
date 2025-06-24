---
title: GraphMockResponsePlugin
description: GraphMockResponsePlugin reference
author: garrytrinder
ms.author: garrytrinder
ms.date: 04/08/2024
---

# GraphMockResponsePlugin

Mocks responses to Microsoft Graph APIs. Includes all functionality of the [MockResponsePlugin](./MockResponsePlugin.md) and adds support for mocking Microsoft Graph API batch requests.

:::image type="content" source="../media/microsoft-graph-mock-response.png" alt-text="Screenshot of a command prompt with Dev Proxy mocking a response to a Microsoft Graph API." lightbox="../media/microsoft-graph-mock-response.png":::

## Plugin instance definition

```json
{
  "name": "GraphMockResponsePlugin",
  "enabled": true,
  "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
  "configSection": "mocksPlugin"
}
```

## Configuration example

See [MockResponsePlugin](./MockResponsePlugin.md)

## Configuration properties

See [MockResponsePlugin](./MockResponsePlugin.md)

## Command line options

See [MockResponsePlugin](./MockResponsePlugin.md)
