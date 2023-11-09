---
title: RateLimitingPlugin
description: RateLimitingPlugin reference
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

# RateLimitingPlugin

Simulates rate-limit behaviors.

## Plugin instance definition

```json
{
  "name": "RateLimitingPlugin",
  "enabled": false,
  "pluginPath": "plugins\\m365-developer-proxy-plugins.dll",
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

| Property                | Description                                                                                      |          Default           |
| ----------------------- | ------------------------------------------------------------------------------------------------ | :------------------------: |
| HeaderLimit             | Name of the response header that communicates the rate-limiting limit                            |     `RateLimit-Limit`      |
| HeaderRemaining         | Name of the response header that communicates the remaining number of resources before the reset |   `RateLimit-Remaining`    |
| HeaderReset             | Name of the response header that communicates the time remaining until the reset                 |     `RateLimit-Reset`      |
| HeaderRetryAfter        | Name of the response header that communicates the retry-after period                             |       `Retry-After`        |
| CostPerRequest          | How many resources does a request cost                                                           |             2              |
| ResetTimeWindowSeconds  | How long in seconds until the next reset                                                         |             60             |
| WarningThresholdPercent | The percentage of use that's when exceeded starts returning rate limiting response headers     |             80             |
| RateLimit               | Number of resources for a time window                                                            |            120             |
| RetryAfterSeconds       | Number of seconds to wait before retrying the request when throttled                             |             5              |
| WhenLimitExceeded       | The behavior the plugin should use when limit is exceeded. Use `Throttle` or `Custom`.          |         `Throttle`         |
| ResetFormat             | The format used to determine when the rate limit resets. Use `SecondsLeft` or `UtcEpochSeconds`. |       `SecondsLeft`        |
| CustomResponseFile      | File containing a custom error response used when limit is exceeded.                             | `rate-limit-response.json` |
| CustomResponse          | Custom error response used when limit is exceeded.                                               |                            |

## Command line options

None
