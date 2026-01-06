---
title: Intercept requests to localhost
description: How to configure Dev Proxy to intercept requests to localhost in Chromium browsers
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/03/2026
zone_pivot_groups: client-operating-system
---

<!-- INTENT: Configure browsers to proxy localhost -->
<!-- SOLUTION: Use browser-specific proxy bypass settings -->
<!-- RESULT: Localhost requests intercepted by Dev Proxy -->
<!-- PLUGINS: none -->
<!-- JOB: intercept-requests -->
<!-- TIME: 5 minutes -->

# Intercept requests to `localhost`

> **At a glance**  
> **Goal:** Configure browsers to proxy localhost  
> **Time:** 5 minutes  
> **Plugins:** None  
> **Prerequisites:** [Set up Dev Proxy](../get-started/set-up.md)

By default, Chromium-based browsers such as Microsoft Edge and Google Chrome bypass system proxy settings for `localhost` URLs. If you're developing an application that makes requests to API on `localhost`, which you want to intercept using Dev Proxy, you need to exclude `localhost` URLs from the bypass list.

To configure Chromium-based browsers to send requests to the system proxy for `localhost` URLs, you need to start the browser with the `--proxy-bypass-list` and `--proxy-server` options. For example, to exclude `localhost` from the bypass list in Microsoft Edge, start the browser with the following command:

::: zone pivot="client-operating-system-windows"

```console
msedge --proxy-bypass-list="<-loopback>" --proxy-server="127.0.0.1:8000"
```

::: zone-end

::: zone pivot="client-operating-system-macos"

```console
/Applications/Microsoft\ Edge.app/Contents/MacOS/Microsoft\ Edge --proxy-bypass-list="<-loopback>" --proxy-server="127.0.0.1:8000"
```

::: zone-end

::: zone pivot="client-operating-system-linux"

```console
/opt/microsoft/msedge-dev/msedge --proxy-bypass-list="<-loopback>" --proxy-server="127.0.0.1:8000"
```

::: zone-end

> [!IMPORTANT]
> Before you start a Chromium-based browser with these settings, be sure to close all instances of the browser. Otherwise, the new settings won't take effect.

To configure Mozilla Firefox to send requests to the system proxy for `localhost` URLs, you need to set the `network.proxy.allow_hijacking_localhost` preference to `true`. To do that, open the `about:config` page in Firefox, search for the `network.proxy.allow_hijacking_localhost` preference, and set it to `true`.

## See also

- [Configure Dev Proxy](../get-started/configure.md)
- [Dev Proxy doesn't intercept requests](./no-requests-intercepted.md)
- [Intercept requests with specific headers](./intercept-requests-specific-headers.md)
