---
title: Plugin architecture
description: The architecture of Dev Proxy plugins
author: garrytrinder
ms.author: garrytrinder
ms.date: 01/16/2024
---

# Plugin architecture

A plugin is a piece of code (DLL) that determines proxy behavior. The proxy executes the plugin code at runtime. Developers can write custom plugins to provide behaviors of their own custom APIs.

The [`devproxyrc.json`](../technical-reference/devproxyrc.md) file contains the plugin configuration.

The proxy has standard plugins that can be used with any API, and plugins specifically for use with the Microsoft Graph API.

The standard plugins are:

- **MockResponsePlugin** responds to requests with a mocked response.
- **RateLimitingPlugin** simulates behaviors of APIs that return `Rate-Limit` headers in responses.
- **ODataPluginGuidance** provides guidance to help developers correctly handle retrieving multiple pages of data.
- **ExecutionSummaryPlugin** exports proxy activity to a summary report.
- **GenericRandomErrorPlugin** fails requests.

Example configuration of the `MockResponsePlugin`:

```json
{
  "name": "MockResponsePlugin",
  "enabled": true,
  "pluginPath": "~appFolder/plugins/dev-proxy-plugins.dll",
  "configSection": "mocksPlugin"
}
```

The `configSection` takes a reference to a configuration object. The `mocksPlugin` object describes the name of the file containing mocked responses.

```json
"mocksPlugin": {
  "mocksFile": "mocks.json"
}
```

The five Microsoft Graph API plugins provided are:

- **GraphRandomErrorPlugin** provides random error responses for Microsoft Graph. It returns a random error response from a list of [supported HTTP codes](../technical-reference/Supported-HTTP-error-status-codes.md).
- **GraphSdkGuidancePlugin** provides guidance when a Microsoft Graph SDK isn't used.
- **GraphSelectGuidancePlugin** provides guidance when the `$select` query-string parameter isn't used on a GET request.
- **GraphBetaSupportGuidancePlugin** provides guidance when a Microsoft Graph v1.0 endpoint isn't used.
- **GraphClientRequestIdGuidancePlugin** provides guidance when the `client-request-id` request header isn't used.

Example configuration of the `GraphSelectGuidancePlugin`:

```json
{
  "name": "GraphBetaSupportGuidancePlugin",
  "enabled": true,
  "pluginPath": "~appFolder/plugins/dev-proxy-plugins.dll",
  "urlsToWatch": [
    "https://graph.microsoft.com/beta/*",
    "https://graph.microsoft.us/beta/*",
    "https://dod-graph.microsoft.us/beta/*",
    "https://microsoftgraph.chinacloudapi.cn/beta/*"
  ]
}
```

Plugins watch requests made to URLs in the global `urlsToWatch` array. But, each plugin can override this list for their own needs. In the above example, the `GraphBetaSupportGuidancePlugin` watches only Microsoft Graph Beta endpoint URLs.
