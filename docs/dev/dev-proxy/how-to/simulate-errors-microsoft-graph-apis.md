---
title: Simulate errors from Microsoft Graph APIs
description: How to configure Dev Proxy to simulate errors from Microsoft Graph APIs
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/06/2026
---

<!-- INTENT: Test how your app handles Microsoft Graph API errors -->
<!-- SOLUTION: Enable GraphRandomErrorPlugin with Graph error responses -->
<!-- RESULT: App receives realistic Graph API errors -->
<!-- PLUGINS: GraphRandomErrorPlugin -->
<!-- JOB: test-error-handling -->
<!-- TIME: 10 minutes -->

# Simulate errors from Microsoft Graph APIs

> **At a glance**  
> **Goal:** Test how your app handles Microsoft Graph API errors  
> **Time:** 10 minutes  
> **Plugins:** [GraphRandomErrorPlugin](../technical-reference/graphrandomerrorplugin.md)  
> **Prerequisites:** [Set up Dev Proxy](../get-started/set-up.md)

Microsoft Graph is a collection of APIs that give you access to data and insights on Microsoft 365. When you use Microsoft Graph in your app, you should test how your app handles API errors. Dev Proxy allows you to simulate errors on any Microsoft Graph API using the [`GraphRandomErrorPlugin`](../technical-reference/GraphRandomErrorPlugin.md).

The `GraphRandomErrorPlugin` is optimized to work with Microsoft Graph and simulates the specific errors that Microsoft Graph can return.

To start, enable the `GraphRandomErrorPlugin` in your configuration file.

**File:** devproxyrc.json

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.0.0/rc.schema.json",
  "plugins": [
    {
      "name": "GraphRandomErrorPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "graphRandomErrorPlugin",
      "urlsToWatch": [
        "https://graph.microsoft.com/v1.0/*",
        "https://graph.microsoft.com/beta/*",
        "https://graph.microsoft.us/v1.0/*",
        "https://graph.microsoft.us/beta/*",
        "https://dod-graph.microsoft.us/v1.0/*",
        "https://dod-graph.microsoft.us/beta/*",
        "https://microsoftgraph.chinacloudapi.cn/v1.0/*",
        "https://microsoftgraph.chinacloudapi.cn/beta/*"
      ]
    }
  ]
}
```

> [!TIP]
> The above snippet listens for requests to Microsoft Graph in all Microsoft clouds. If you only want to simulate errors in a specific Microsoft cloud, remove the URLs that you don't need.

Start Dev Proxy with your configuration file and use your app to see how it handles the errors. For each matching request, Dev Proxy determines whether to simulate an error or pass the request through to Microsoft Graph using the [configured failure rate](./change-request-failure-rate.md). When Dev Proxy simulates an error, it randomly selects one of the errors that Microsoft Graph uses, and returns an error response to your app.

## Configure errors to simulate

By default, the `GraphRandomErrorPlugin` simulates the following errors.

| HTTP method | Possible errors |
|-------------|-----------------|
| `GET` | `429 Too Many Requests`, `500 Internal Server Error`, `502 Bad Gateway`, `503 Service Unavailable`, `504 Gateway Timeout` |
| `POST` | `429 Too Many Requests`, `500 Internal Server Error`, `502 Bad Gateway`, `503 Service Unavailable`, `504 Gateway Timeout`, `507 Insufficient Storage` |
| `PUT` | `429 Too Many Requests`, `500 Internal Server Error`, `502 Bad Gateway`, `503 Service Unavailable`, `504 Gateway Timeout`, `507 Insufficient Storage` |
| `PATCH` | `429 Too Many Requests`, `500 Internal Server Error`, `502 Bad Gateway`, `503 Service Unavailable`, `504 Gateway Timeout` |
| `DELETE` | `429 Too Many Requests`, `500 Internal Server Error`, `502 Bad Gateway`, `503 Service Unavailable`, `504 Gateway Timeout`, `507 Insufficient Storage` |

If you want to test specific behaviors, such as throttling, configure the plugin to only use the relevant errors using the `--allowed-errors` option.

```console
devproxy --allowed-errors 429
```

Alternatively, you can configure the `allowedErrors` property in the `graphRandomErrorPlugin` object in your configuration file.

**File:** devproxyrc.json

```json
{
  "graphRandomErrorPlugin": {
    "allowedErrors": [ 429 ]
  }
}
```

## Simulate errors in Microsoft Graph batch requests

Dev Proxy simulates errors in batch requests to Microsoft Graph in the same way as it does for regular requests. When Dev Proxy fails one or more requests in a batch request, it returns a `424 Failed Dependency` response for the whole batch request, just like Microsoft Graph would.

## See also

- [GraphRandomErrorPlugin](../technical-reference/graphrandomerrorplugin.md) - Full reference
- [Simulate throttling on Microsoft 365 APIs](./simulate-throttling-microsoft-365.md) - Throttling-specific testing
- [Test my app with random errors](./test-my-app-with-random-errors.md) - Generic error simulation
- [Glossary](../concepts/glossary.md) - Dev Proxy terminology
