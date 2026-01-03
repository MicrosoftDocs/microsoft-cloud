---
title: Why do all requests fail with gateway timeout
description: How to fix all requests failing with gateway timeout
author: garrytrinder
ms.author: garrytrinder
ms.date: 01/03/2026
---

<!-- INTENT: Fix gateway timeout on first run -->
<!-- SOLUTION: Trust Dev Proxy root certificate -->
<!-- RESULT: Requests pass through proxy successfully -->
<!-- PLUGINS: none -->
<!-- JOB: troubleshoot -->
<!-- TIME: 2 minutes -->

# Why do all requests fail with gateway timeout

> **At a glance**  
> **Goal:** Fix gateway timeout on first run  
> **Time:** 2 minutes  
> **Plugins:** None  
> **Prerequisites:** [Set up Dev Proxy](../get-started/set-up.md)

If you're running the proxy for the first time, it can happen that the network access configuration didn't propagate timely and the proxy started without access to your network.

Close the proxy by pressing <kbd>Ctrl</kbd> + <kbd>C</kbd> in proxy's window and restart the proxy and it should be working as intended.

## See also

- [Get help and support](./get-help-and-support.md)
- [Why is my internet connection not working after using the proxy](./why-is-my-internet-connection-not-working-after-using-the-proxy.md)
- [No requests intercepted](./no-requests-intercepted.md)
