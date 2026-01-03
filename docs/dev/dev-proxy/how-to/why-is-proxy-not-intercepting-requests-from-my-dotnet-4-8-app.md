---
title: Why is proxy not intercepting requests from my .NET 4.8 app
description: How to configure the Dev Proxy for use with .NET 4.8 apps
author: garrytrinder
ms.author: garrytrinder
ms.date: 01/03/2026
---

<!-- INTENT: Fix Dev Proxy with .NET 4.8 -->
<!-- SOLUTION: Configure app.config with defaultProxy element -->
<!-- RESULT: .NET 4.8 app requests routed through proxy -->
<!-- PLUGINS: various -->
<!-- JOB: intercept-requests -->
<!-- TIME: 5 minutes -->

# Why is proxy not intercepting requests from my .NET 4.8 app

> **At a glance**  
> **Goal:** Fix Dev Proxy with .NET 4.8  
> **Time:** 5 minutes  
> **Plugins:** Various  
> **Prerequisites:** [Set up Dev Proxy](../get-started/set-up.md)

To use `devproxy` with .NET 4.8, you need to configure it as proxy with the `WinHTTP API`.

Use the `netsh` command to configure the proxy. (run as admin):

```console
netsh winhttp set proxy proxy-server="http=localhost:8000;https=localhost:8000"
```

When you're done, you can reset the WinHTTP settings by using:

```console
netsh winhttp reset proxy
```

## See also

- [Use Dev Proxy with .NET applications](./use-dev-proxy-with-dotnet.md)
- [No requests intercepted](./no-requests-intercepted.md)
- [Intercept requests from specific processes](./intercept-requests-from-specific-process.md)
