---
title: Detect minimal Microsoft Graph API permissions
description: How to detect the minimal Microsoft Graph API permissions that your app requires
author: garrytrinder
ms.author: garrytrinder
ms.date: 01/03/2026
---

<!-- INTENT: Find the minimum permissions needed for Microsoft Graph API calls -->
<!-- SOLUTION: Enable GraphMinimalPermissionsPlugin and record API calls -->
<!-- RESULT: Report showing minimal permissions required for recorded calls -->
<!-- PLUGINS: GraphMinimalPermissionsPlugin -->
<!-- JOB: check-permissions -->

# Detect minimal Microsoft Graph API permissions

> **At a glance**  
> **Goal:** Find the minimal permissions your app needs for Microsoft Graph  
> **Time:** 10 minutes  
> **Plugins:** [GraphMinimalPermissionsPlugin](../technical-reference/graphminimalpermissionsplugin.md)  
> **Prerequisites:** [Set up Dev Proxy](../get-started/set-up.md)

Microsoft Graph exposes hundreds of endpoints that allow you to tap into data and insights in Microsoft 365. To use these API endpoints, you need to request a correct set of permissions.

If you work on a large solution that uses many endpoints, it can be difficult to build the exact list of minimal permissions for your application.

To detect the minimal Microsoft Graph API permissions that your app requires:

1. Enable the [`GraphMinimalPermissionsPlugin`](../technical-reference/graphminimalpermissionsplugin.md) plugin.
1. [Start recording](./Record-and-export-proxy-activity.md).
1. Use your app to issue requests as normal.
1. [Stop recording](./Record-and-export-proxy-activity.md).

The proxy returns a list of minimal permissions in the activity summary based on the intercepted requests.

For example:

```console
Retrieving minimal permissions for:
- GET /me
- GET /users/{users-id}/calendars

Minimal permissions:
User.Read, Calendars.Read
```

By default, Dev Proxy detects minimal `Delegated` permissions.

To return `Application` permissions, update the `graphMinimalPermissionsPlugin` configuration block in the [devproxyrc.json](../technical-reference/devproxyrc.md) file to:

**File:** devproxyrc.json

```json
{
  "graphMinimalPermissionsPlugin": {
    "type": "application"
  }
}
```

## See also

- [Check if you're using excessive Microsoft Graph API permissions](./check-if-you-are-using-excessive-microsoft-graph-api-permissions.md) - Compare your permissions to minimal set
- [Record and export proxy activity](./record-and-export-proxy-activity.md) - How to start and stop recording
- [GraphMinimalPermissionsPlugin reference](../technical-reference/graphminimalpermissionsplugin.md) - Configuration options
- [Glossary](../concepts/glossary.md) - Dev Proxy terminology
