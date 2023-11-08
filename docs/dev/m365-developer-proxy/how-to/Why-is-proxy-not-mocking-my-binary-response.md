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

The dollar sign has special meaning in some shells and might need to be escaped.

In PowerShell, use a backtick:

```pwsh
Invoke-WebRequest -Method GET -Uri "https://graph.microsoft.com/v1.0/users/id/photo/`$value" -Proxy http://localhost:8000
```

In bash, wrap the URL in double quotes and add a backslash:

```sh
curl -ikx http://localhost:8000 "https://graph.microsoft.com/v1.0/users/id/photo/\$value"
```
