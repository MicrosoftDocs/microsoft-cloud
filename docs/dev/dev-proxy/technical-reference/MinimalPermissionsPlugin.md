---
title: MinimalPermissionsPlugin
description: MinimalPermissionsPlugin reference
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

# MinimalPermissionsPlugin

Returns a list of the minimal permissions required for Microsoft Graph requests that proxy recorded.

:::image type="content" source="../media/microsoft-graph-minimal-permissions.png" alt-text="Dev Proxy showing a list of minimal permissions for the set of Microsoft Graph requests" lightbox="../media/microsoft-graph-minimal-permissions.png":::

## Plugin instance definition

```json
{
  "name": "MinimalPermissionsPlugin",
  "enabled": false,
  "pluginPath": "plugins\\dev-proxy-plugins.dll",
  "configSection": "minimalPermissionsPlugin"
}
```

## Configuration example

```json
{
  "minimalPermissionsPlugin": {
    "type": "delegated"
  }
}
```

## Configuration properties

| Property | Description | Default |
|----------|-------------|:-------:|
| `type` | Determines which type of permission scopes to return. Can be `Delegated` or `Application` | `Delegated` |

## Command line options

None
