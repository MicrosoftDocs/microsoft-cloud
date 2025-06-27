---
title: GraphMinimalPermissionsGuidancePlugin
description: GraphMinimalPermissionsGuidancePlugin reference
author: garrytrinder
ms.author: garrytrinder
ms.date: 04/30/2025
---

# GraphMinimalPermissionsGuidancePlugin

Compares the permissions used in the JWT token sent to Microsoft Graph against the minimum required scopes needed for requests that proxy recorded and shows the difference.

:::image type="content" source="../media/microsoft-graph-minimal-permissions-guidance.png" alt-text="Screenshot of a command prompt with Dev Proxy showing minimal permissions for a set of Microsoft Graph API requests." lightbox="../media/microsoft-graph-minimal-permissions-guidance.png":::

## Plugin instance definition

```json
{
  "name": "GraphMinimalPermissionsGuidancePlugin",
  "enabled": false,
  "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
  "configSection": "graphMinimalPermissionsGuidancePlugin"
}
```

## Configuration example

```json
{
  "graphMinimalPermissionsGuidancePlugin": {
   "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v0.29.2/graphminimalpermissionsguidanceplugin.schema.json",
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

## Next step

> [!div class="nextstepaction"]
> [Check if you're using excessive Microsoft Graph API permissions](../how-to/check-if-you-are-using-excessive-microsoft-graph-api-permissions.md)
