---
title: MinimalPermissionsGuidancePlugin
description: MinimalPermissionsGuidancePlugin reference
author: garrytrinder
ms.author: garrytrinder
ms.date: 08/13/2024
---

# MinimalPermissionsGuidancePlugin

Compares the permissions used in the JWT token sent to Microsoft Graph against the minimum required scopes needed for requests that proxy recorded and shows the difference.

:::image type="content" source="../media/microsoft-graph-minimal-permissions-guidance.png" alt-text="Screenshot of a command prompt with Dev Proxy showing minimal permissions for a set of Microsoft Graph API requests." lightbox="../media/microsoft-graph-minimal-permissions-guidance.png":::

## Plugin instance definition

```json
{
  "name": "MinimalPermissionsGuidancePlugin",
  "enabled": false,
  "pluginPath": "~appFolder/plugins/dev-proxy-plugins.dll",
  "configSection": "minimalPermissionsGuidancePlugin"
}
```

## Configuration example

```json
{
    "minimalPermissionsGuidancePlugin": {
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
