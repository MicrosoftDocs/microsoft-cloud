---
title: RateLimitingPlugin
description: RateLimitingPlugin reference
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
  - tool=devproxy
---

# RateLimitingPlugin

Simulates rate-limit behaviors.

:::image type="content" source="../media/rate-limiting-plugin.png" alt-text="Screenshot of a terminal with Dev Proxy simulating rate limiting on GitHub APIs." lightbox="../media/rate-limiting-plugin.png":::

## Plugin instance definition

```json
{
  "name": "RateLimitingPlugin",
  "enabled": false,
  "pluginPath": "plugins\\dev-proxy-plugins.dll",
  "configSection": "rateLimiting"
}
```

## Configuration example

```json
{
  "rateLimiting": {
    "costPerRequest": 2,
    "rateLimit": 120,
    "retryAfterSeconds": 5
  }
}
```

## Configuration properties

| Property                  | Description                                                                                      |          Default           |
| ------------------------- | ------------------------------------------------------------------------------------------------ | :------------------------: |
| `headerLimit`             | Name of the response header that communicates the rate-limiting limit                            |     `RateLimit-Limit`      |
| `headerRemaining`         | Name of the response header that communicates the remaining number of resources before the reset |   `RateLimit-Remaining`    |
| `headerReset`             | Name of the response header that communicates the time remaining until the reset                 |     `RateLimit-Reset`      |
| `headerRetryAfter`        | Name of the response header that communicates the retry-after period                             |       `Retry-After`        |
| `costPerRequest`          | How many resources does a request cost                                                           |             2              |
| `resetTimeWindowSeconds`  | How long in seconds until the next reset                                                         |             60             |
| `warningThresholdPercent` | The percentage of use that's when exceeded starts returning rate limiting response headers     |             80             |
| `rateLimit`               | Number of resources for a time window                                                            |            120             |
| `retryAfterSeconds`       | Number of seconds to wait before retrying the request when throttled                             |             5              |
| `whenLimitExceeded`       | The behavior the plugin should use when limit is exceeded. Use `Throttle` or `Custom`.          |         `Throttle`         |
| `resetFormat`             | The format used to determine when the rate limit resets. Use `SecondsLeft` or `UtcEpochSeconds`. |       `SecondsLeft`        |
| `customResponseFile`      | File containing a custom error response used when limit is exceeded.                             | `rate-limit-response.json` |
| `customResponse`          | Custom error response used when limit is exceeded.                                               |                            |

## Command line options

None
