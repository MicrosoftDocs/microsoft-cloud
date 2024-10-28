---
title: Check if you are using excessive Microsoft Graph API permissions
description: How to check if your app is using minimal Microsoft Graph API permissions
author: garrytrinder
ms.author: garrytrinder
ms.date: 10/28/2024
---

# Check if you're using excessive Microsoft Graph API permissions

Microsoft Graph exposes hundreds of endpoints that allow you to tap into data and insights in Microsoft 365. To use these API endpoints, you need to request a correct set of permissions.

A common approach to security is to apply the principle of least privilege (PoLP). This principle applies to users, processes and programs.

To check if your app is using more permissions than it needs:

1. Enable the [`GraphMinimalPermissionsGuidancePlugin`](../technical-reference/graphminimalpermissionsguidanceplugin.md) plugin.
1. [Start recording](./Record-and-export-proxy-activity.md).
1. Use your app to issue requests as normal.
1. [Stop recording](./Record-and-export-proxy-activity.md).

Dev Proxy returns a list of permissions scopes that are unnecessary in the activity summary based on the intercepted requests.

For example:

```text
Evaluating delegated permissions for:

- GET /me

Permissions on the token:
AllSites.FullControl, User.Read

    WARNING: The following permissions are unnecessary:
    WARNING: AllSites.FullControl
```
