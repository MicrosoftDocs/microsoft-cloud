---
title: Why is proxy not mocking my binary response
description: How to fix proxy not mocking binary responses
author: garrytrinder
ms.author: garrytrinder
ms.date: 01/03/2026
---

<!-- INTENT: Fix binary mocking with special chars -->
<!-- SOLUTION: Use @-file syntax instead of inline body -->
<!-- RESULT: Binary responses mocked correctly -->
<!-- PLUGINS: MockResponsePlugin -->
<!-- JOB: troubleshoot -->
<!-- TIME: 3 minutes -->

# Why is proxy not mocking my binary response

> **At a glance**  
> **Goal:** Fix binary mocking with special chars  
> **Time:** 3 minutes  
> **Plugins:** [MockResponsePlugin](../technical-reference/mockresponseplugin.md)  
> **Prerequisites:** [Set up Dev Proxy](../get-started/set-up.md)

The dollar sign has special meaning in some shells and might need to be escaped.

In PowerShell, use a backtick:

```powershell
Invoke-WebRequest -Method GET -Uri "https://graph.microsoft.com/v1.0/users/id/photo/`$value" -Proxy http://localhost:8000
```

In bash, wrap the URL in double quotes and add a backslash:

```console
curl -ikx http://localhost:8000 "https://graph.microsoft.com/v1.0/users/id/photo/\$value"
```

## See also

- [Mock responses that return binary data](./mock-responses-that-return-binary-data.md)
- [MockResponsePlugin](../technical-reference/mockresponseplugin.md)
- [Mock multiple responses to the same endpoint](./mock-multiple-responses-to-the-same-endpoint.md)
