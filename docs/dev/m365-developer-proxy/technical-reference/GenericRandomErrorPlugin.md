---
title: Get started
description: Get started with Microsoft 365 Developer Proxy
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

Fails requests with a random selected error from file containing mocked errors.

## Plugin instance definition

```json
{
  "name": "GenericRandomErrorPlugin",
  "enabled": true,
  "pluginPath": "plugins\\m365-developer-proxy-plugins.dll",
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
    "rate": 90,
    "errorsFile": "errors.json"
  },
}
```

## Configuration properties

| Property | Description | Default |
|----------|-------------|:-------:|
| Rate | Determines the percentage chance for a request to fail with an error. | No default |
| ErrorsFile | Path to the file that contains error responses. | No default |

## Command line options

None
