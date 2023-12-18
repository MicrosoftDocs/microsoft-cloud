---
title: Change logging level
description: How to change logging level
author: garrytrinder
ms.author: garrytrinder
ms.date: 11/03/2023
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

```sh
devproxy --log-level debug
```
