---
title: Change request failure rate
description: How to configure the request failure rate
author: garrytrinder
ms.author: garrytrinder
ms.date: 01/24/2024
---

# Change request failure rate

By default, there's 50% chance that Dev Proxy returns a random error for your API. You can change the likelihood to a different value using the `--failure-rate` option, for example:

```sh
devproxy --failure-rate 80
```

Alternatively, you can configure the failure rate in the Dev Proxy configuration file.

```json
{
  "$schema": "https://raw.githubusercontent.com/microsoft/dev-proxy/main/schemas/v1.0/rc.schema.json",
  "rate": 80
}
```

> [!IMPORTANT]
> When you configure the failure rate to 0, Dev Proxy will pass all requests through to the original API. When you configure it to 100, Dev Proxy will simulate an error for every matching request.
