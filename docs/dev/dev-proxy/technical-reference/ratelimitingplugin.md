---
title: RateLimitingPlugin
description: RateLimitingPlugin reference
author: garrytrinder
ms.author: garrytrinder
ms.date: 04/30/2025
---

# RateLimitingPlugin

Simulates rate-limit behaviors.

:::image type="content" source="../media/rate-limiting-plugin.png" alt-text="Screenshot of a command prompt with Dev Proxy simulating rate limiting on GitHub APIs." lightbox="../media/rate-limiting-plugin.png":::

## Plugin instance definition

```json
{
  "name": "RateLimitingPlugin",
  "enabled": false,
  "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
  "configSection": "rateLimiting"
}
```

## Configuration example

```json
{
  "rateLimiting": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v0.27.0/ratelimitingplugin.schema.json",
    "costPerRequest": 2,
    "rateLimit": 120
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
| `whenLimitExceeded`       | The behavior the plugin should use when limit is exceeded. Use `Throttle` or `Custom`.          |         `Throttle`         |
| `resetFormat`             | The format used to determine when the rate limit resets. Use `SecondsLeft` or `UtcEpochSeconds`. |       `SecondsLeft`        |
| `customResponseFile`      | File containing a custom error response used when limit is exceeded.                             | `rate-limit-response.json` |

## Command line options

None

## Next step

> [!div class="nextstepaction"]
> [Simulate Rate-Limit API responses](../how-to/simulate-rate-limit-api-responses.md)
