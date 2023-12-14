---
title: GraphMockResponsePlugin
description: GraphMockResponsePlugin reference
author: garrytrinder
ms.author: garrytrinder
ms.date: 11/03/2023
ms.topic: reference
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

# GraphMockResponsePlugin

Mocks responses to Microsoft Graph APIs. Includes all functionality of the [MockResponsePlugin](./MockResponsePlugin.md) and adds support for mocking Microsoft Graph API batch requests.

:::image type="content" source="../media/microsoft-graph-mock-response.png" alt-text="Screenshot of a terminal with Dev Proxy mocking a response to a Microsoft Graph API." lightbox="../media/microsoft-graph-mock-response.png":::

## Plugin instance definition

```json
{
  "name": "GraphMockResponsePlugin",
  "enabled": true,
  "pluginPath": "plugins\\dev-proxy-plugins.dll",
  "configSection": "mocksPlugin"
}
```

## Configuration example

See [MockResponsePlugin](./MockResponsePlugin.md)

## Configuration properties

See [MockResponsePlugin](./MockResponsePlugin.md)

## Command line options

See [MockResponsePlugin](./MockResponsePlugin.md)
