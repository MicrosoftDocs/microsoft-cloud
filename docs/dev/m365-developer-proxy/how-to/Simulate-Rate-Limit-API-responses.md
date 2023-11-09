---
title: Simulate Rate-Limit API responses
description: How to simulate Rate-Limit API responses
author: garrytrinder
ms.author: garrytrinder
ms.contributors: garrytrinder
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

# Simulate Rate-Limit API responses

Rate-Limit headers are used in HTTP responses to limit the number of requests that a client can make within a given time period.

The server sends these headers in response to a client’s request to indicate how many requests are allowed and how many requests remain before the limit is reached.

The `RateLimit-Limit` response header field indicates the request-quota associated with the client in the current time-window.

If the client exceeds that limit, it might not be served.

> ℹ️ By default the [RateLimitingPlugin](../technical-reference/RateLimitingPlugin.md) is disabled. To enable the plugin, open the [m365proxyrc](https://github.com/microsoft/m365-developer-proxy/wiki/m365proxyrc) file, search for the plugin object and change the `enabled` property to `true`. The plugin will be enabled the next time you start proxy.

Enabling the plugin loads presets to simulate the Rate Limiting behaviors of Microsoft Graph and SharePoint Online APIs.

## Custom rate limit support

In v0.12, we added support for proxy to work with custom Rate-Limit implementations.

The following example shows how you can configure how you can configure [RateLimitingPlugin](../technical-reference/RateLimitingPlugin.md) in the [m365proxyrc](../technical-reference/m365proxyrc.md) file to simulate rate limits for the [GitHub API](https://docs.github.com/en/rest/guides/best-practices-for-using-the-rest-api?apiVersion=2022-11-28#dealing-with-rate-limits).

```json
{
  "rateLimiting": {
    "headerLimit": "X-RateLimit-Limit",
    "headerRemaining": "X-RateLimit-Remaining",
    "headerReset": "X-RateLimit-Reset",
    "costPerRequest": 1,
    "resetTimeWindowSeconds": 3600,
    "warningThresholdPercent": 0,
    "rateLimit": 60,
    "resetFormat": "UtcEpochSeconds",
    "whenLimitExceeded": "Custom",
    "customResponseFile": "github-rate-limit-exceeded.json"
  }
}
```

The `customResponseFile` contains the response that the proxy returns when your app reached the rate limit.

```json
{
  "responseCode": 403,
  "responseHeaders": {
    "Content-Type": "application/json; charset=utf-8"
  },
  "responseBody": {
    "message": "You have exceeded a secondary rate limit and have been temporarily blocked from content creation. Please retry your request again later.",
    "documentation_url": "https://docs.github.com/rest/overview/resources-in-the-rest-api#secondary-rate-limits"
  }
}
```
