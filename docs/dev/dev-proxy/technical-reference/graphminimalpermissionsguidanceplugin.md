---
title: GraphMinimalPermissionsGuidancePlugin
description: GraphMinimalPermissionsGuidancePlugin reference
author: garrytrinder
ms.author: garrytrinder
ms.date: 10/28/2024
---

# GraphMinimalPermissionsGuidancePlugin

Compares the permissions used in the JWT token sent to Microsoft Graph against the minimum required scopes needed for requests that proxy recorded and shows the difference.

:::image type="content" source="../media/microsoft-graph-minimal-permissions-guidance.png" alt-text="Screenshot of a command prompt with Dev Proxy showing minimal permissions for a set of Microsoft Graph API requests." lightbox="../media/microsoft-graph-minimal-permissions-guidance.png":::

## Plugin instance definition

```json
{
  "name": "GraphMinimalPermissionsGuidancePlugin",
  "enabled": false,
  "pluginPath": "~appFolder/plugins/dev-proxy-plugins.dll",
  "configSection": "graphMinimalPermissionsGuidancePlugin"
}
```

## Configuration example

```json
{
  "graphMinimalPermissionsGuidancePlugin": {
    "permissionsToIgnore": [ 
      "profile", 
      "openid", 
      "offline_access", 
      "email"
    ]
  }
}
```

## Configuration properties

| Property | Description | Default |
| -------- | ----------- | :-----: |
| `permissionsToIgnore` | The scopes to ignore and not include in the report. | `profile openid offline_access email` |

## Command line options

None
