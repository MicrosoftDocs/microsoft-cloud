---
title: Intercept requests to localhost
description: How to configure Dev Proxy to intercept requests to localhost in Chromium browsers
author: waldekmastykarz
ms.author: wmastyka
ms.date: 05/27/2024
zone_pivot_groups: client-operating-system
---

# Intercept requests to `localhost`

By default, Chromium-based browsers such as Microsoft Edge and Google Chrome bypass system proxy settings for `localhost` URLs. If you're developing an application that makes requests to API on `localhost`, which you want to intercept using Dev Proxy, you need to exclude `localhost` URLs from the bypass list.

To configure Chromium-based browsers to send requests to the system proxy for `localhost` URLs, you need to start the browser with the `--proxy-bypass-list` option. For example, to exclude `localhost` from the bypass list in Microsoft Edge, start the browser with the following command:

::: zone pivot="client-operating-system-windows"

```console
msedge --proxy-bypass-list="<-loopback>"
```

::: zone-end

::: zone pivot="client-operating-system-macos"

```console
/Applications/Microsoft\ Edge.app/Contents/MacOS/Microsoft\ Edge --proxy-bypass-list="<-loopback>"
```

::: zone-end
