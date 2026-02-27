---
title: Troubleshoot network interference
description: Learn how to fix issues where Dev Proxy interferes with other applications
author: waldekmastykarz
ms.author: wmastyka
ms.date: 02/26/2026
ms.topic: how-to
zone_pivot_groups: client-operating-system
---

<!-- INTENT: Fix apps losing connectivity when Dev Proxy runs -->
<!-- SOLUTION: Configure proxy exceptions or disable system proxy mode -->
<!-- RESULT: Apps work normally while Dev Proxy intercepts target traffic -->
<!-- PLUGINS: none -->
<!-- JOB: troubleshoot -->
<!-- TIME: 5 minutes -->

# Troubleshoot network interference

> **At a glance**  
> **Goal:** Fix apps losing connectivity when Dev Proxy runs  
> **Time:** 5 minutes  
> **Plugins:** None  
> **Prerequisites:** [Set up Dev Proxy](../get-started/set-up.md)

Some applications lose network connectivity while Dev Proxy is running, even when their URLs aren't in your `urlsToWatch` list. This article explains why it happens and how to fix it.

## Symptoms

- Applications like Outlook, WhatsApp, or other desktop apps stop working when Dev Proxy starts
- Apps show connection errors even though their URLs aren't being watched
- Network connectivity returns immediately when you stop Dev Proxy

## Cause

When Dev Proxy registers itself as a system proxy (the default behavior), all HTTP/HTTPS traffic on your machine flows through it. Some applications use **certificate pinning**, meaning they only accept connections signed by specific certificates. Because Dev Proxy uses its own certificate to intercept traffic, these apps reject the connection and lose network access.

This issue is most common on Windows, where applications automatically use the system proxy settings.

## Solutions

Choose the solution that best fits your workflow:

### Option 1: Exclude URLs in Windows proxy settings

Add exceptions for problematic domains in Windows proxy settings. Traffic to these domains bypasses Dev Proxy entirely.

1. Open **Settings** > **Network & Internet** > **Proxy**
1. Under **Manual proxy setup**, find **Use the proxy server except for addresses that start with the following entries**
1. Add the domains that are having issues, separated by semicolons. For example:

   ```text
   *.outlook.com;*.office.com;*.whatsapp.com;*.whatsapp.net
   ```

1. Select **Save**

Traffic to these domains now bypasses Dev Proxy while other traffic still flows through it.

### Option 2: Disable system proxy mode

If you don't need Dev Proxy to intercept traffic from all applications, disable the system proxy and configure only your target application to use the proxy.

#### Using command line

Start Dev Proxy without registering as the system proxy:

```console
devproxy --as-system-proxy false
```

#### Using configuration file

Add this setting to your `devproxyrc.json`:

```json
{
  "asSystemProxy": false
}
```

Then configure your application to use Dev Proxy by setting the `https_proxy` environment variable:

::: zone pivot="client-operating-system-windows"

```console
set https_proxy=http://127.0.0.1:8000
```

::: zone-end

::: zone pivot="client-operating-system-macos"

```console
export https_proxy=http://127.0.0.1:8000
```

::: zone-end

::: zone pivot="client-operating-system-linux"

```console
export https_proxy=http://127.0.0.1:8000
```

::: zone-end

With this setup, only applications that respect the `https_proxy` environment variable send traffic through Dev Proxy. Other applications connect directly to the internet.

### Option 3: Watch specific processes only

Use the `watchProcessNames` or `watchPids` settings to intercept traffic only from specific applications. This approach still registers Dev Proxy as a system proxy but limits which processes have their traffic intercepted.

```json
{
  "watchProcessNames": ["node", "dotnet"]
}
```

## See also

- [Proxy settings](../technical-reference/proxy-settings.md)
- [Dev Proxy doesn't intercept requests](no-requests-intercepted.md)
- [Why is my internet connection not working after using the proxy](why-is-my-internet-connection-not-working-after-using-the-proxy.md)
