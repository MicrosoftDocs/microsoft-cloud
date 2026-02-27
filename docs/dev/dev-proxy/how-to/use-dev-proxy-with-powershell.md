---
title: Use Dev Proxy with PowerShell
description: Learn how to use Dev Proxy with PowerShell on Windows, macOS, and Linux
author: waldekmastykarz
ms.author: wmastyka
ms.date: 02/27/2026
ms.topic: how-to
---

<!-- INTENT: Use Dev Proxy with PowerShell on all operating systems -->
<!-- SOLUTION: Restart PowerShell or set proxy manually (Windows); no extra steps needed (macOS/Linux) -->
<!-- RESULT: PowerShell HTTP requests routed through Dev Proxy -->
<!-- JOB: intercept-requests -->
<!-- TIME: 5 minutes -->

# Use Dev Proxy with PowerShell

> **At a glance**  
> **Goal:** Configure PowerShell to work with Dev Proxy  
> **Time:** 5 minutes  
> **Prerequisites:** [Set up Dev Proxy](../get-started/set-up.md)

Dev Proxy works with PowerShell on all operating systems. On macOS and Linux, PowerShell correctly detects system proxy changes and no extra configuration is needed. On Windows, PowerShell caches proxy settings, which requires extra steps.

## macOS and Linux

On macOS and Linux, PowerShell detects system proxy changes automatically. Start Dev Proxy and use PowerShell as you normally would—requests are intercepted without any extra configuration.

## Windows

When you use Dev Proxy with PowerShell on Windows, you might encounter issues where PowerShell doesn't detect when the system proxy starts or stops. This issue affects both Windows PowerShell (5.1) and PowerShell 7+ (Core).

### The issue

On Windows, PowerShell sessions cache the system proxy settings when they start. This means that if you:

1. Open a PowerShell session
1. Start Dev Proxy (which configures itself as the system proxy)
1. Make HTTP requests from the original PowerShell session

The requests aren't routed through Dev Proxy because PowerShell uses the cached proxy settings from when the session started.

Similarly, if you:

1. Start Dev Proxy
1. Open a PowerShell session and make requests (they're intercepted correctly)
1. Stop Dev Proxy
1. Make requests from the same PowerShell session

The requests fail because PowerShell still tries to use the proxy that no longer exists.

### Workaround: Restart PowerShell session

The simplest workaround is to restart your PowerShell session after starting or stopping Dev Proxy. Restarting the session ensures PowerShell picks up the current system proxy settings.

### Alternative: Set the proxy manually

Instead of relying on system proxy detection, you can configure Dev Proxy to not register as the system proxy and set the proxy manually in PowerShell.

#### Step 1: Start Dev Proxy without system proxy

Start Dev Proxy with the `--as-system-proxy false` option:

```console
devproxy --as-system-proxy false
```

#### Step 2: Set the proxy environment variable in PowerShell

In your PowerShell session, set the `HTTPS_PROXY` environment variable:

```powershell
$env:HTTPS_PROXY = "http://127.0.0.1:8000"
```

If you also need to intercept HTTP requests:

```powershell
$env:HTTP_PROXY = "http://127.0.0.1:8000"
```

#### Step 3: Clear the proxy when done

When you're done using Dev Proxy, clear the environment variables:

```powershell
$env:HTTPS_PROXY = $null
$env:HTTP_PROXY = $null
```

### Use a PowerShell profile

To simplify setting the proxy, you can create functions in your PowerShell profile. Add the following to your `$PROFILE` file:

```powershell
function Set-DevProxy {
    $env:HTTP_PROXY = "http://127.0.0.1:8000"
    $env:HTTPS_PROXY = "http://127.0.0.1:8000"
    Write-Host "Dev Proxy enabled"
}

function Clear-DevProxy {
    $env:HTTP_PROXY = $null
    $env:HTTPS_PROXY = $null
    Write-Host "Dev Proxy disabled"
}
```

Then you can use `Set-DevProxy` and `Clear-DevProxy` commands to quickly enable or disable the proxy in your PowerShell session.
