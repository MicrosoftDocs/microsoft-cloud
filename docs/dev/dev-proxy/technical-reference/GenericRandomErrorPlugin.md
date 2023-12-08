---
title: GenericRandomErrorPlugin
description: GenericRandomErrorPlugin reference
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

# GenericRandomErrorPlugin

Fails requests with a random selected error from file containing mocked errors.

:::image type="content" source="../media/generic-random-error-plugin.png" alt-text="Screenshot of a terminal with the Dev Proxy simulating one of the errors for an OpenAI API request as defined in the config file." lightbox="../media/generic-random-error-plugin.png":::

## Plugin instance definition

```json
{
  "name": "GenericRandomErrorPlugin",
  "enabled": true,
  "pluginPath": "plugins\\dev-proxy-plugins.dll",
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
    "errorsFile": "errors.json"
  }
}
```

## Configuration properties

| Property | Description | Default |
|----------|-------------|:-------:|
| `errorsFile` | Path to the file that contains error responses. | No default |

## Command line options

None
