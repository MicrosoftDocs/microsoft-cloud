---
title: Intercept requests with specific headers
description: How to configure Dev Proxy to only intercept requests with specific headers
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/06/2026
---

<!-- INTENT: Filter by specific headers -->
<!-- SOLUTION: Configure filterByHeaders in devproxyrc.json -->
<!-- RESULT: Only requests with matching headers are intercepted -->
<!-- PLUGINS: none -->
<!-- JOB: intercept-requests -->
<!-- TIME: 5 minutes -->

# Intercept requests with specific headers

> **At a glance**  
> **Goal:** Filter by specific headers  
> **Time:** 5 minutes  
> **Plugins:** None  
> **Prerequisites:** [Set up Dev Proxy](../get-started/set-up.md)

By default, Dev Proxy intercepts all requests that match the URLs configured in the [devproxyrc.json](../technical-reference/devproxyrc.md) file. When you want to intercept only specific requests, such as requests issued by a specific component, you can configure Dev Proxy to intercept requests with specific headers.

To intercept requests with specific headers, in the `devproxyrc.json` file, add the `filterByHeaders` property. In the `filterByHeaders` property, specify the headers that you want to use to filter the requests. For each header, specify the value that the header should contain for Dev Proxy to intercept the request. If you leave the value empty, Dev Proxy intercepts requests that contain the specified header, regardless of its value.

## Example: Intercept requests with a specific header and value

The following example demonstrates how to configure Dev Proxy to intercept requests that contain the `x-app` header with a value that contains `contoso-intranet`:

**File:** `devproxyrc.json`

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/rc.schema.json",
  "plugins": [
    {
      "name": "GenericRandomErrorPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll"
    }
  ],
  "urlsToWatch": [
    "https://api.contoso.com/*"
  ],
  "filterByHeaders": [
    {
      "name": "x-app",
      "value": "contoso-intranet"
    }
  ]
}
```

Using this configuration, Dev Proxy intercepts requests that contain the `x-app` header with the value `contoso-intranet`, for example:

```http
GET https://api.contoso.com/customers
x-app: contoso-intranet
```

Dev Proxy also intercepts requests that partially match the specified value, for example:

```http
GET https://api.contoso.com/customers
x-app: contoso-intranet-search
```

Dev Proxy doesn't intercept the following request because the value of the `x-app` header doesn't contain `contoso-intranet`:

```http
GET https://api.contoso.com/customers
x-app: contoso-public
```

Partial matching is convenient and allows you to intercept requests with values that can change over time, such as component or SDK version.

## Example: Intercept requests with a specific header no matter the value

To intercept requests that contain a specific header, regardless of its value, leave the value empty:

**File:** `devproxyrc.json`

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/rc.schema.json",
  "plugins": [
    {
      "name": "GenericRandomErrorPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll"
    }
  ],
  "urlsToWatch": [
    "https://api.contoso.com/*"
  ],
  "filterByHeaders": [
    {
      "name": "x-contoso",
      "value": ""
    }
  ]
}
```

Using this configuration, Dev Proxy intercepts requests that contain the `x-contoso` header, regardless of its value:

```http
GET https://api.contoso.com/customers
x-contoso: api-sdk v1.0
```

Or:

```http
GET https://api.contoso.com/customers
x-contoso: intranet
```

Dev Proxy doesn't intercept the following request because it doesn't have the `x-contoso` header:

```http
GET https://api.contoso.com/customers
x-app: contoso-public
```

## See also

- [Configure Dev Proxy](../get-started/configure.md)
- [Intercept requests from specific processes](./intercept-requests-from-specific-processes.md)
- [Discover URLs to watch](./discover-urls-watch.md)
