---
title: Get started
description: Get started with Microsoft 365 Developer Proxy
author: garrytrinder
ms.author: garrytrinder
ms.contributors: garrytrinder
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

Microsoft Graph exposes hundreds of endpoints that allow you to tap into data and insights in Microsoft 365. To use these API endpoints, you need to request a correct set of permissions.

A common approach to security is to apply the principle of least privilege (PoLP). This principle is applied to users, processes and programs.

To check if your app is using more permissions than it needs:

> ℹ️ By default the `MinimalPermissionsGuidancePlugin` is disabled. To enable the plugin, open the `m365proxyrc.json` file, search for the plugin object and change the `enabled` property to `true`. The plugin will be enabled the next time you start proxy.

1. [Start recording](./Record-and-export-proxy-activity.md).
1. Issue requests as normal from your app.
1. [Stop recording](./Record-and-export-proxy-activity.md).

The proxy returns a list of permissions scopes that are unnecessary in the activity summary based on the intercepted requests.

For example:

```sh
Evaluating delegated permissions for:

- GET /me

Permissions on the token:
AllSites.FullControl, User.Read

    WARNING: The following permissions are unnecessary:
    WARNING: AllSites.FullControl
```
