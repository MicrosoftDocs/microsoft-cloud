---
title: Console output text labels
description: How to change console output to use text labels
author: garrytrinder
ms.author: garrytrinder
ms.date: 10/21/2026
---

<!-- INTENT: Understand Dev Proxy console output labels and their meanings -->

# Console output text labels

Dev Proxy uses the following text labels in the console output.

| Label | Description |
| ----- | ------------ |
|`req`| Proxy intercepted a request |
|`res`| Proxy intercepted a response |
|`pass`| Proxy passed the request through |
|`oops`| Proxy simulated an API error |
|`warn`| Proxy returned a performance warning |
|`mock`| Proxy mocked the response |
|`fail`| Request failed |
|`tip`| Proxy provided developer guidance |
|`skip`| Plugin skipped the request |
|`proc`| Plugin processed the request |
|`time`| Timestamp of the request |

For log entries that aren't related to a specific request, Dev Proxy uses labels that reflect the log level.

| Label | Log level |
| ----- | ------------ |
|`trce`| Trace |
|`dbug`| Debug |
|`info`| Information |
|`warn`| Warning |
|`fail`| Error |
|`crit`| Critical |
