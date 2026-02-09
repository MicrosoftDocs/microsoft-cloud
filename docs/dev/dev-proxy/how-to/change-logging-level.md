---
title: Change logging level
description: How to change logging level
author: garrytrinder
ms.author: garrytrinder
ms.date: 02/07/2026
---

<!-- INTENT: Adjust Dev Proxy logging verbosity -->
<!-- SOLUTION: Use --log-level option or logLevel config setting -->
<!-- RESULT: Dev Proxy outputs more or less detail in console -->
<!-- JOB: configure-proxy -->
<!-- TIME: 2 minutes -->

# Change logging level

> **At a glance**  
> **Goal:** Adjust Dev Proxy logging verbosity to troubleshoot issues or reduce noise  
> **Time:** 2 minutes  
> **Prerequisites:** [Set up Dev Proxy](../get-started/set-up.md)

By default, the proxy is set to log information level events. These events are messages not related to requests, only the events related to the working of Dev Proxy.

To change the default logging level, update the `logLevel` setting in the configuration file. Dev Proxy automatically reloads its configuration when you save changes, so you don't need to restart the proxy.

**File:** `devproxyrc.json`  
**Purpose:** Set persistent logging level

```json
{
  "logLevel": "debug"
}
```

To change the logging level at run time, use the `--log-level` command line option.

```console
devproxy --log-level debug
```

For the list of available logging levels, see the [Proxy settings](../technical-reference/proxy-settings.md) page.

## See also

- [Proxy settings](../technical-reference/proxy-settings.md) - All logging levels
- [Glossary](../concepts/glossary.md) - Dev Proxy terminology
