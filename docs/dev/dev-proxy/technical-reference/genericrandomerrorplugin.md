---
title: GenericRandomErrorPlugin
description: GenericRandomErrorPlugin reference
author: garrytrinder
ms.author: garrytrinder
ms.date: 04/30/2025
---

# GenericRandomErrorPlugin

Fails requests with a random selected error from file containing mocked errors.

:::image type="content" source="../media/generic-random-error-plugin.png" alt-text="Screenshot of a command prompt with the Dev Proxy simulating one of the errors for an OpenAI API request as defined in the config file." lightbox="../media/generic-random-error-plugin.png":::

## Plugin instance definition

```json
{
  "name": "GenericRandomErrorPlugin",
  "enabled": true,
  "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
  "configSection": "genericRandomErrorPlugin",
  "urlsToWatch": [
    "https://api.openai.com/*"
  ]
}
```

## Configuration example

```json
{
  "genericRandomErrorPlugin": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v0.29.0/genericrandomerrorplugin.schema.json",
    "errorsFile": "errors.json"
  }
}
```

## Configuration properties

| Property | Description | Default |
|----------|-------------|:-------:|
| `errorsFile` | Path to the file that contains error responses. | No default |
| `rate` | The percentage of requests to fail with a random error. Value between 0 and 100. | `50` |
| `retryAfterInSeconds` | The number of seconds to wait before retrying the request. Included on the `Retry-After` response header for dynamic throttling. | `5` |

## Command line options

| Name | Description | Default |
|----------|-------------|:-------:|
| `-f, --failure-rate <failure rate>` | The percentage of requests to fail with a random error. Value between 0 and 100. | `50` |

## Next step

> [!div class="nextstepaction"]
> [Test my app with random errors](../how-to/test-my-app-with-random-errors.md)
