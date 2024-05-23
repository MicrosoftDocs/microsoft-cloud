---
title: MinimalPermissionsGuidancePlugin
description: MinimalPermissionsGuidancePlugin reference
author: garrytrinder
ms.author: garrytrinder
ms.date: 05/22/2024
---

# MinimalPermissionsGuidancePlugin

Compares the permissions used in the JWT token sent to Microsoft Graph against the minimum required scopes needed for requests that proxy recorded and shows the difference.

:::image type="content" source="../media/microsoft-graph-minimal-permissions-guidance.png" alt-text="Screenshot of a command prompt with Dev Proxy showing minimal permissions for a set of Microsoft Graph API requests." lightbox="../media/microsoft-graph-minimal-permissions-guidance.png":::

## Plugin instance definition

```json
{
  "name": "MinimalPermissionsGuidancePlugin",
  "enabled": false,
  "pluginPath": "~appFolder/plugins/dev-proxy-plugins.dll"
}
```

## Configuration example

None

## Configuration properties

None

## Command line options

None
