---
title: OpenApiSpecGeneratorPlugin
description: OpenApiSpecGeneratorPlugin reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 06/10/2024
---

# OpenApiSpecGeneratorPlugin

Generates OpenAPI spec in JSON format from the intercepted requests and responses.

:::image type="content" source="../media/open-api-spec-generator-plugin.png" alt-text="Screenshot of two command prompt windows. One shows Dev Proxy recording API requests. The other shows the generated OpenAPI spec." lightbox="../media/open-api-spec-generator-plugin.png":::

## Plugin instance definition

```json
{
  "name": "OpenApiSpecGeneratorPlugin",
  "enabled": true,
  "pluginPath": "~appFolder/plugins/dev-proxy-plugins.dll",
  "configSection": "openApiSpecGeneratorPlugin"
}
```

## Configuration example

```json
{
  "openApiSpecGeneratorPlugin": {
    "includeOptionsRequests": false
  }
}
```

## Configuration properties

| Property | Description | Default |
| -------- | ----------- | :-----: |
| `includeOptionsRequests` | Determines whether to include `OPTIONS` requests in the generated OpenAPI spec | `false` |

## Command line options

None

## Remarks

To create better OpenAPI specs, consider using a local language model with Dev Proxy. Using a local language model, the `OpenApiSpecGeneratorPlugin` generates clearer operation IDs and descriptions, giving you a better starting point for your OpenAPI spec. To use a local language model with the `OpenApiSpecGeneratorPlugin`, enable the language model in the configuration file. For more information, see [Use a local language model](../how-to/use-language-model.md).
