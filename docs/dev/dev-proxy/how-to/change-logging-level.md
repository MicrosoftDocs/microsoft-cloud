---
title: Change logging level
description: How to change logging level
author: garrytrinder
ms.author: garrytrinder
ms.date: 04/08/2024
---

# Change logging level

By default, the proxy is set to log information level events. These events are messages not related to requests, only the events related to the working of Dev Proxy.

To change the default logging level, update the `logLevel` setting in the `devproxyrc.json` file.

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
