---
title: GraphMinimalPermissionsPlugin
description: GraphMinimalPermissionsPlugin reference
author: garrytrinder
ms.author: garrytrinder
ms.date: 02/27/2026
---

<!-- INTENT: Detect minimal permissions needed for Microsoft Graph calls -->
<!-- PLUGIN-TYPE: Reporting -->
<!-- WORKS-WITH: GraphMinimalPermissionsGuidancePlugin, MarkdownReporter, JsonReporter -->
<!-- USE-WHEN: Auditing Microsoft Graph permissions before production -->

# GraphMinimalPermissionsPlugin

Returns a list of the minimal permissions required for Microsoft Graph requests that proxy recorded.

:::image type="content" source="../media/microsoft-graph-minimal-permissions.png" alt-text="Screenshot of a command prompt with Dev Proxy showing a list of minimal permissions for the set of Microsoft Graph requests." lightbox="../media/microsoft-graph-minimal-permissions.png":::

## Configuration example

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.2.0/rc.schema.json",
  "plugins": [
    {
      "name": "GraphMinimalPermissionsPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "graphMinimalPermissionsPlugin"
    }
  ],
  "graphMinimalPermissionsPlugin": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.2.0/graphminimalpermissionsplugin.schema.json",
    "type": "delegated"
  }
}
```

## Configuration properties

| Property | Description | Default |
|----------|-------------|:-------:|
| `type` | Determines which type of permission scopes to return. Can be `delegated` or `application` | `delegated` |

## Command line options

None

## Next step

> [!div class="nextstepaction"]
> [Detect minimal Microsoft Graph API permissions](../how-to/detect-minimal-microsoft-graph-api-permissions.md)
