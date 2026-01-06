---
title: Why is my internet connection not working after using the proxy
description: Why is my internet connection not working after using the proxy
author: garrytrinder
ms.author: garrytrinder
ms.date: 01/03/2026
---

<!-- INTENT: Restore network after proxy crash -->
<!-- SOLUTION: Disable system proxy in OS settings -->
<!-- RESULT: Network connectivity restored -->
<!-- PLUGINS: none -->
<!-- JOB: troubleshoot -->
<!-- TIME: 2 minutes -->

# Why is my internet connection not working after using the proxy

> **At a glance**  
> **Goal:** Restore network after proxy crash  
> **Time:** 2 minutes  
> **Plugins:** None  
> **Prerequisites:** [Set up Dev Proxy](../get-started/set-up.md)

If you terminate the Dev Proxy process, the proxy doesn't unregister itself and your network connection on your machine is blocked.

To restore network connection, start the proxy and close it by pressing <kbd>Ctrl</kbd> + <kbd>C</kbd>. Closing proxy this way gracefully closes the proxy unregistering it on your machine and restoring the regular network connection.

## See also

- [Get help and support](./get-help-and-support.md)
- [Why do all requests fail with gateway timeout](./why-do-all-requests-fail-with-gateway-timeout.md)
- [Uninstall Dev Proxy](./uninstall.md)
