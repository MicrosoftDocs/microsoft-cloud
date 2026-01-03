---
title: Check if you're using excessive Microsoft Graph API permissions
description: How to check if your app is using minimal Microsoft Graph API permissions
author: garrytrinder
ms.author: garrytrinder
ms.date: 01/03/2026
---

<!-- INTENT: Identify unnecessary Microsoft Graph permissions in your app -->
<!-- SOLUTION: Enable GraphMinimalPermissionsGuidancePlugin to analyze requests -->
<!-- RESULT: Report showing excessive permissions and recommendations -->
<!-- PLUGINS: GraphMinimalPermissionsGuidancePlugin -->
<!-- JOB: check-permissions -->
<!-- TIME: 10 minutes -->

# Check if you're using excessive Microsoft Graph API permissions

> **At a glance**  
> **Goal:** Identify unnecessary Microsoft Graph permissions in your app  
> **Time:** 10 minutes  
> **Plugins:** [GraphMinimalPermissionsGuidancePlugin](../technical-reference/graphminimalpermissionsguidanceplugin.md)  
> **Prerequisites:** [Set up Dev Proxy](../get-started/set-up.md)

Microsoft Graph exposes hundreds of endpoints that allow you to tap into data and insights in Microsoft 365. To use these API endpoints, you need to request a correct set of permissions.

A common approach to security is to apply the principle of least privilege (PoLP). This principle applies to users, processes, and programs.

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

## See also

- [Detect minimal Microsoft Graph API permissions](./detect-minimal-microsoft-graph-api-permissions.md) - Find minimal permissions needed
- [Record and export proxy activity](./record-and-export-proxy-activity.md) - How to start and stop recording
- [GraphMinimalPermissionsGuidancePlugin](../technical-reference/graphminimalpermissionsguidanceplugin.md) - Full reference
- [Glossary](../concepts/glossary.md) - Dev Proxy terminology
