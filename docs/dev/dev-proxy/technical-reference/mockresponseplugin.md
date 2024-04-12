---
title: MockResponsePlugin
description: MockResponsePlugin reference
author: garrytrinder
ms.author: garrytrinder
ms.date: 04/12/2024
---

# MockResponsePlugin

Simulates responses.

:::image type="content" source="../media/mock-response-plugin.png" alt-text="Screenshot of a command prompt with Dev Proxy simulating response for a request to GitHub API." lightbox="../media/mock-response-plugin.png":::

## Plugin instance definition

```json
{
  "name": "MockResponsePlugin",
  "enabled": true,
  "pluginPath": "~appFolder/plugins/dev-proxy-plugins.dll",
  "configSection": "mocksPlugin"
}
```

## Configuration example

```json
{
  "mocksPlugin": {
    "mocksFile": "mocks.json"
  }
}
```

## Configuration properties

| Property              | Description                                                        |     Default      |
| --------------------- | ------------------------------------------------------------------ | :--------------: |
| `mocksFile`             | Path to the file containing mock responses                         | `mocks.json` |
| `blockUnmockedRequests` | Return `502 Bad Gateway` response for requests that aren't mocked |     `false`      |

## Command line options

| Name             | Description                                | Default |
| ---------------- | ------------------------------------------ | :-----: |
| `-n, --no-mocks` | Disable loading mock requests              | `false` |
| `--mocks-file`   | Path to the file containing mock responses |    -    |

## More information

- [Mock object](../technical-reference/response-object.md)
