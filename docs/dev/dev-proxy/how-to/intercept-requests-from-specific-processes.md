---
title: Intercept requests from specific processes
description: How to configure the proxy to intercept requests only from specific processes
author: garrytrinder
ms.author: garrytrinder
ms.date: 01/03/2026
---

<!-- INTENT: Filter Dev Proxy to only intercept requests from specific apps -->
<!-- SOLUTION: Use --watch-pids or --watch-process-names options -->
<!-- RESULT: Only requests from specified processes are intercepted -->
<!-- JOB: intercept-requests -->
<!-- TIME: 3 minutes -->

# Intercept requests from specific processes

> **At a glance**  
> **Goal:** Filter Dev Proxy to only intercept requests from specific apps  
> **Time:** 3 minutes  
> **Prerequisites:** [Set up Dev Proxy](../get-started/set-up.md)

By default, Dev Proxy is registered as a system wide proxy and all requests made by your machine are passed through the proxy.

By default, the proxy intercepts requests that are made from your machine to the URLs configured in [devproxyrc.json](../technical-reference/devproxyrc.md) file.

However, you might also want to only intercept requests being made from specific processes such as a command prompt window or web browser.

To intercept request from processes by their given process IDs, use the `--watch-pids` option:

```console
devproxy –-watch-pids 870 135100
```

To intercept request from processes by their given process names, use the `--watch-process-names` option:

```console
devproxy --watch-process-names msedge pwsh
```

## See also

- [Discover URLs to watch](./discover-urls-watch.md) - Filter by process during discovery
- [Glossary](../concepts/glossary.md) - Dev Proxy terminology
