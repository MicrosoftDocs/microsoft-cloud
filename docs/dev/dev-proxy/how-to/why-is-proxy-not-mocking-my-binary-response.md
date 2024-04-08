---
title: Why is proxy not mocking my binary response
description: How to fix proxy not mocking binary responses
author: garrytrinder
ms.author: garrytrinder
ms.date: 04/08/2024
---

# Why is proxy not mocking my binary response

The dollar sign has special meaning in some shells and might need to be escaped.

In PowerShell, use a backtick:

```powershell
Invoke-WebRequest -Method GET -Uri "https://graph.microsoft.com/v1.0/users/id/photo/`$value" -Proxy http://localhost:8000
```

In bash, wrap the URL in double quotes and add a backslash:

```console
curl -ikx http://localhost:8000 "https://graph.microsoft.com/v1.0/users/id/photo/\$value"
```
