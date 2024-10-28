---
title: GraphMinimalPermissionsPlugin
description: GraphMinimalPermissionsPlugin reference
author: garrytrinder
ms.author: garrytrinder
ms.date: 10/28/2024
---

# GraphMinimalPermissionsPlugin

Returns a list of the minimal permissions required for Microsoft Graph requests that proxy recorded.

:::image type="content" source="../media/microsoft-graph-minimal-permissions.png" alt-text="Screenshot of a command prompt with Dev Proxy showing a list of minimal permissions for the set of Microsoft Graph requests." lightbox="../media/microsoft-graph-minimal-permissions.png":::

## Plugin instance definition

```json
{
  "name": "GraphMinimalPermissionsPlugin",
  "enabled": false,
  "pluginPath": "~appFolder/plugins/dev-proxy-plugins.dll",
  "configSection": "graphMinimalPermissionsPlugin"
}
```

## Configuration example

```json
{
  "graphMinimalPermissionsPlugin": {
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
