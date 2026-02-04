---
title: GraphMinimalPermissionsGuidancePlugin
description: GraphMinimalPermissionsGuidancePlugin reference
author: garrytrinder
ms.author: garrytrinder
ms.date: 01/06/2026
---

<!-- INTENT: Compare JWT permissions against minimal required scopes -->
<!-- PLUGIN-TYPE: Reporting -->
<!-- WORKS-WITH: GraphMinimalPermissionsPlugin, MarkdownReporter, JsonReporter -->
<!-- USE-WHEN: Auditing excessive Microsoft Graph permissions in production apps -->

# GraphMinimalPermissionsGuidancePlugin

Compares the permissions used in the JWT token sent to Microsoft Graph against the minimum required scopes needed for requests that proxy recorded and shows the difference.

:::image type="content" source="../media/microsoft-graph-minimal-permissions-guidance.png" alt-text="Screenshot of a command prompt with Dev Proxy showing minimal permissions for a set of Microsoft Graph API requests." lightbox="../media/microsoft-graph-minimal-permissions-guidance.png":::

## Configuration example

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/rc.schema.json",
  "plugins": [
    {
      "name": "GraphMinimalPermissionsGuidancePlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "graphMinimalPermissionsGuidancePlugin"
    }
  ],
  "graphMinimalPermissionsGuidancePlugin": {
   "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/graphminimalpermissionsguidanceplugin.schema.json",
    "permissionsToExclude": [ 
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
| `permissionsToExclude` | The scopes to ignore and not include in the report. | `profile openid offline_access email` |

## Command line options

None

## Next step

> [!div class="nextstepaction"]
> [Check if you're using excessive Microsoft Graph API permissions](../how-to/check-if-you-are-using-excessive-microsoft-graph-api-permissions.md)
