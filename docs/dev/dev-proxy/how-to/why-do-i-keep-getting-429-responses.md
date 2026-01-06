---
title: Why do I keep getting 429 responses
description: How to fix all requests failing with a 429 response
author: garrytrinder
ms.author: garrytrinder
ms.date: 01/03/2026
---

<!-- INTENT: Understand constant 429 errors -->
<!-- SOLUTION: Disable GenericRandomErrorPlugin or adjust rate -->
<!-- RESULT: Fewer or no simulated 429 errors -->
<!-- PLUGINS: various -->
<!-- JOB: troubleshoot -->
<!-- TIME: 2 minutes -->

# Why do I keep getting 429 responses

> **At a glance**  
> **Goal:** Understand constant 429 errors  
> **Time:** 2 minutes  
> **Plugins:** Various  
> **Prerequisites:** [Set up Dev Proxy](../get-started/set-up.md)

If you have the failure rate at `100%`, then no request can ever succeed.

If you configure a lower failure rate, then a previously throttled request is passed to the API provided the `Retry-After` time period elapses.

## See also

- [Change request failure rate](./change-request-failure-rate.md)
- [Simulate throttling](./simulate-throttling-microsoft-365.md)
- [What is throttling?](../concepts/what-is-throttling.md)
