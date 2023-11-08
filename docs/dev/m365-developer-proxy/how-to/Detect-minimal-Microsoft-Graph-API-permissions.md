---
title: Detect minimal Microsoft Graph API permissions
description: How to detect the minimal Microsoft Graph API permissions that your app requires
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

If you work on a large solution that uses many endpoints, it can be difficult to build the exact list of minimal permissions for your application.

To detect the minimal Microsoft Graph API permissions that your app requires:

1. [Start recording](./Record-and-export-proxy-activity.md).
1. Issue requests as normal from your app.
1. [Stop recording](./Record-and-export-proxy-activity.md).

The proxy returns a list of minimal permissions in the activity summary based on the intercepted requests.

For example:

```sh
Retrieving minimal permissions for:
- GET /me
- GET /users/{users-id}/calendars

Minimal permissions:
User.Read, Calendars.Read
```

By default, only `Delegated` permissions are returned in the summary.

To return `Application` permissions, update the `minimalPermissionsPlugin` configuration block in the [m365proxyrc](../technical-reference/m365proxyrc.md) file to:

```json
"minimalPermissionsPlugin": {
  "type": "application"
},
```
