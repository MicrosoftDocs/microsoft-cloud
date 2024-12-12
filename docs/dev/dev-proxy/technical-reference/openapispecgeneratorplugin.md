---
title: OpenApiSpecGeneratorPlugin
description: OpenApiSpecGeneratorPlugin reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 12/12/2024
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
    "includeOptionsRequests": false,
    "specVersion": "v3_0",
    "specFormat": "Json"
  }
}
```

## Configuration properties

| Property | Description | Default |
| -------- | ----------- | :-----: |
| `includeOptionsRequests` | Determines whether to include `OPTIONS` requests in the generated OpenAPI spec | `false` |
| `specVersion` | Determines which version to use for the generated OpenAPI spec. Can be set to `v2_0` or `v3_0` | `v3_0` |
| `specFormat` | Determines which format to use for the generated OpenAPI spec. Can be set to `Json` or `Yaml` | `Json` |

## Command line options

None

## Remarks

To create better OpenAPI specs, consider using a local language model with Dev Proxy. Using a local language model, the `OpenApiSpecGeneratorPlugin` generates clearer operation IDs and descriptions, giving you a better starting point for your OpenAPI spec. To use a local language model with the `OpenApiSpecGeneratorPlugin`, enable the language model in the configuration file. For more information, see [Use a local language model](../how-to/use-language-model.md).
