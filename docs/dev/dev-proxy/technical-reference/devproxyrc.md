---
title: devproxyrc
description: devproxyrc.json file reference
author: garrytrinder
ms.author: garrytrinder
ms.date: 11/03/2023
ms.topic: conceptual
ms.service: microsoft-cloud-for-developers

categories:
  - developer-tools
products:
  - microsoft-365
  - microsoft-graph
  - sharepoint-online
  - m365
ms.custom:
  - fcp
  - team=cloud_advocates
---

# devproxyrc

Default configuration.

```json
{
  "plugins": [
    {
      "name": "LatencyPlugin",
      "enabled": false,
      "pluginPath": "plugins\\dev-proxy-plugins.dll",
      "configSection": "latencyPlugin"
    },
    {
      "name": "RetryAfterPlugin",
      "enabled": true,
      "pluginPath": "plugins\\dev-proxy-plugins.dll"
    },
    {
      "name": "GraphSelectGuidancePlugin",
      "enabled": true,
      "pluginPath": "plugins\\dev-proxy-plugins.dll",
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
    },
    {
      "name": "GraphBetaSupportGuidancePlugin",
      "enabled": true,
      "pluginPath": "plugins\\dev-proxy-plugins.dll",
      "urlsToWatch": [
        "https://graph.microsoft.com/beta/*",
        "https://graph.microsoft.us/beta/*",
        "https://dod-graph.microsoft.us/beta/*",
        "https://microsoftgraph.chinacloudapi.cn/beta/*"
      ]
    },
    {
      "name": "GraphSdkGuidancePlugin",
      "enabled": true,
      "pluginPath": "plugins\\dev-proxy-plugins.dll",
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
    },
    {
      "name": "ODataPagingGuidancePlugin",
      "enabled": true,
      "pluginPath": "plugins\\dev-proxy-plugins.dll"
    },
    {
      "name": "GraphClientRequestIdGuidancePlugin",
      "enabled": true,
      "pluginPath": "plugins\\dev-proxy-plugins.dll",
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
    },
    {
      "name": "CachingGuidancePlugin",
      "enabled": true,
      "pluginPath": "plugins\\dev-proxy-plugins.dll",
      "configSection": "cachingGuidance"
    },
    {
      "name": "RateLimitingPlugin",
      "enabled": false,
      "pluginPath": "plugins\\dev-proxy-plugins.dll",
      "configSection": "rateLimiting"
    },
    {
      "name": "MockResponsePlugin",
      "enabled": false,
      "pluginPath": "plugins\\dev-proxy-plugins.dll",
      "configSection": "mocksPlugin"
    },
    {
      "name": "GraphMockResponsePlugin",
      "enabled": true,
      "pluginPath": "plugins\\dev-proxy-plugins.dll",
      "configSection": "mocksPlugin"
    },
    {
      "name": "GraphRandomErrorPlugin",
      "enabled": true,
      "pluginPath": "plugins\\dev-proxy-plugins.dll",
      "configSection": "graphRandomErrorsPlugin"
    },
    {
      "name": "ExecutionSummaryPlugin",
      "enabled": false,
      "pluginPath": "plugins\\dev-proxy-plugins.dll",
      "configSection": "executionSummaryPlugin"
    },
    {
      "name": "MinimalPermissionsPlugin",
      "enabled": true,
      "pluginPath": "plugins\\dev-proxy-plugins.dll",
      "configSection": "minimalPermissionsPlugin"
    },
    {
      "name": "MinimalPermissionsGuidancePlugin",
      "enabled": false,
      "pluginPath": "plugins\\dev-proxy-plugins.dll"
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
    "https://*.sharepoint.*/*_api/*",
    "https://*.sharepoint.*/*_vti_bin/*",
    "https://*.sharepoint-df.*/*_api/*",
    "https://*.sharepoint-df.*/*_vti_bin/*"
  ],
  "mocksPlugin": {
    "mocksFile": "responses.json"
  },
  "graphRandomErrorsPlugin": {
    "allowedErrors": [ 429, 500, 502, 503, 504, 507 ]
  },
  "executionSummaryPlugin": {
    "groupBy": "url"
  },
  "minimalPermissionsPlugin": {
    "type": "delegated"
  },
  "cachingGuidance": {
    "cacheThresholdSeconds": 5
  },
  "latencyPlugin": {
    "minMs": 200,
    "maxMs": 10000
  },
  "rateLimiting": {
    "costPerRequest": 2,
    "rateLimit": 120,
    "retryAfterSeconds": 5
  },
  "rate": 50,
  "labelMode": "text",
  "logLevel": "info"
}
```