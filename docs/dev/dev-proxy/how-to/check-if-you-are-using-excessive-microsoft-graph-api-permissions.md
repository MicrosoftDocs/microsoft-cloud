---
title: Check if you are using excessive Microsoft Graph API permissions
description: How to check if your app is using minimal Microsoft Graph API permissions
author: garrytrinder
ms.author: garrytrinder
ms.date: 11/03/2023
---

# Check if you're using excessive Microsoft Graph API permissions

Microsoft Graph exposes hundreds of endpoints that allow you to tap into data and insights in Microsoft 365. To use these API endpoints, you need to request a correct set of permissions.

A common approach to security is to apply the principle of least privilege (PoLP). This principle applies to users, processes and programs.

To check if your app is using more permissions than it needs:

> [!NOTE]
> By default the `MinimalPermissionsGuidancePlugin` is disabled. To enable the plugin, open the `devproxyrc.json` file, search for the plugin object and change the `enabled` property to `true`. The plugin will be enabled the next time you start proxy.

1. [Start recording](./Record-and-export-proxy-activity.md).
1. Use your app to issue requests as normal.
1. [Stop recording](./Record-and-export-proxy-activity.md).

Dev Proxy returns a list of permissions scopes that are unnecessary in the activity summary based on the intercepted requests.

For example:

```sh
Evaluating delegated permissions for:

- GET /me

Permissions on the token:
AllSites.FullControl, User.Read

    WARNING: The following permissions are unnecessary:
    WARNING: AllSites.FullControl
```
