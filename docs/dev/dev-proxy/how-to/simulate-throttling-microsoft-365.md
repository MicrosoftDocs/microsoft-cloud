---
title: Simulate throttling on Microsoft 365 APIs
description: How to test that your application connected to Microsoft 365 APIs handles throttling properly
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/24/2024
---

# Simulate throttling on Microsoft 365 APIs

Typically, testing [throttling](../concepts/what-is-throttling.md) is hard because it occurs rarely, when Microsoft 365 servers are under heavy load. Using the Dev Proxy, you can simulate throttling responses, and check if your application handles it correctly.

To simulate throttling on Microsoft 365 APIs, use the [GraphRandomErrorPlugin](../technical-reference/graphrandomerrorplugin.md) and the [RetryAfterPlugin](../technical-reference/retryafterplugin.md). The `GraphRandomErrorPlugin` returns throttling responses for Microsoft 365 APIs. The `RetryAfterPlugin` verifies that your app backs-off as instructed by the API.

To start, enable the `GraphRandomErrorPlugin` and `RetryAfterPlugin` in your Dev Proxy configuration file.

```json
{
  "$schema": "https://raw.githubusercontent.com/microsoft/dev-proxy/main/schemas/v1.0/rc.schema.json",
  "plugins": [
    {
      "name": "RetryAfterPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/dev-proxy-plugins.dll"
    },
    {
      "name": "GraphRandomErrorPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/dev-proxy-plugins.dll",
      "configSection": "graphRandomErrorPlugin"
    }
  ],
  "urlsToWatch": [
    "https://graph.microsoft.com/v1.0/*",
    "https://graph.microsoft.com/beta/*",
    "https://graph.microsoft.us/v1.0/*",
    "https://graph.microsoft.us/beta/*",
    "https://dod-graph.microsoft.us/v1.0/*",
    "https://dod-graph.microsoft.us/beta/*",
    "https://microsoftgraph.chinacloudapi.cn/v1.0/*",
    "https://microsoftgraph.chinacloudapi.cn/beta/*",
    "!https://*.sharepoint.*/*_api/web/GetClientSideComponents",
    "https://*.sharepoint.*/*_api/*",
    "https://*.sharepoint.*/*_vti_bin/*",
    "https://*.sharepoint-df.*/*_api/*",
    "https://*.sharepoint-df.*/*_vti_bin/*"
  ]
}
```

> [!CAUTION]
> Add the `RetryAfterPlugin` before the `GraphRandomErrorPlugin` in your configuration file. If you add it after, the request will be failed by the `GraphRandomErrorPlugin` before the `RetryAfterPlugin` has a chance to handle it.

Next, configure the `GraphRandomErrorPlugin` to simulate throttling errors.

```json
{
  "$schema": "https://raw.githubusercontent.com/microsoft/dev-proxy/main/schemas/v1.0/rc.schema.json",
  "plugins": [
    {
      "name": "RetryAfterPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/dev-proxy-plugins.dll"
    },
    {
      "name": "GraphRandomErrorPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/dev-proxy-plugins.dll",
      "configSection": "graphRandomErrorPlugin"
    }
  ],
  "urlsToWatch": [
    "https://graph.microsoft.com/v1.0/*",
    "https://graph.microsoft.com/beta/*",
    "https://graph.microsoft.us/v1.0/*",
    "https://graph.microsoft.us/beta/*",
    "https://dod-graph.microsoft.us/v1.0/*",
    "https://dod-graph.microsoft.us/beta/*",
    "https://microsoftgraph.chinacloudapi.cn/v1.0/*",
    "https://microsoftgraph.chinacloudapi.cn/beta/*",
    "!https://*.sharepoint.*/*_api/web/GetClientSideComponents",
    "https://*.sharepoint.*/*_api/*",
    "https://*.sharepoint.*/*_vti_bin/*",
    "https://*.sharepoint-df.*/*_api/*",
    "https://*.sharepoint-df.*/*_vti_bin/*"
  ],
  "graphRandomErrorPlugin": {
    "allowedErrors": [ 429 ]
  }
}
```

Start Dev Proxy with your configuration file and test your app to see how it handles throttling.

If your application backs-off when throttled, but doesn't wait for the amount of time specified on the requests, you see a message similar to `Calling https://graph.microsoft.com/v1.0/endpoint again before waiting for the Retry-After period. Request will be throttled`.

This message indicates that your application isn't handling throttling correctly and unnecessarily prolongs throttling. To improve how your app handles throttling, update your code to wait for the amount of time specified in the `Retry-After` header before retrying the request.

## More information

- [What is throttling?](../concepts/what-is-throttling.md)
