---
title: Dev Proxy doesn't intercept requests
description: What to do when Dev Proxy is running, but doesn't intercept any requests.
author: waldekmastykarz
ms.author: wmastyka
ms.date: 02/04/2025
---

# Dev Proxy doesn't intercept requests

Dev Proxy is running, but no requests are intercepted. Several reasons can cause this issue.

## Dev Proxy isn't registered as a system proxy

If Dev Proxy isn't registered as a system proxy, it doesn't intercept any requests, unless you explicitly configure your app to use it. If you want Dev Proxy to intercept all requests, make sure it's registered as a system proxy.

After starting Dev Proxy, check your system proxy settings to verify that Dev Proxy is registered as a system proxy.

If you start Dev Proxy and it's not registered as a system proxy, verify its settings. In the command line, open the Dev Proxy config file by running `devproxy config`. Verify, that the file doesn't contain the `asSystemProxy: false` setting.

## URL isn't included in the configuration

If the URL you're trying to intercept isn't included in the configuration, Dev Proxy doesn't intercept requests to it. Make sure the URL is included in the `urlsToWatch` section of the configuration file.

Learn more about [configuring URLs to watch](../get-started/configure.md).

## No requests are intercepted

Dev Proxy is running, is registered as a system proxy, the URL is included in the configuration, but it still doesn't intercept requests. Some apps don't automatically pick up system proxy settings, which can cause this issue. In this case, you need to configure your app to use Dev Proxy explicitly.

See the section on [using Dev Proxy with different apps](./overview.md#use-dev-proxy) in this documentation.
