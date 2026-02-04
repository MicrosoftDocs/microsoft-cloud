---
title: OpenApiSpecGeneratorPlugin
description: OpenApiSpecGeneratorPlugin reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/06/2026
---

<!-- INTENT: Generate OpenAPI spec from intercepted API traffic -->
<!-- PLUGIN-TYPE: Reporting -->
<!-- WORKS-WITH: TypeSpecGeneratorPlugin, MarkdownReporter, JsonReporter -->
<!-- USE-WHEN: Reverse-engineering API specs from undocumented APIs -->

# OpenApiSpecGeneratorPlugin

Generates OpenAPI spec in JSON format from the intercepted requests and responses.

:::image type="content" source="../media/open-api-spec-generator-plugin.png" alt-text="Screenshot of two command prompt windows. One shows Dev Proxy recording API requests. The other shows the generated OpenAPI spec." lightbox="../media/open-api-spec-generator-plugin.png":::

## Configuration example

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/rc.schema.json",
  "plugins": [
    {
      "name": "OpenApiSpecGeneratorPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "openApiSpecGeneratorPlugin"
    }
  ],
  "openApiSpecGeneratorPlugin": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/openapispecgeneratorplugin.schema.json",
    "includeOptionsRequests": false,
    "ignoreResponseTypes": false,
    "specVersion": "v3_0",
    "specFormat": "Json",
    "includeParameters": [
      "api-version"
    ]
  }
}
```

## Configuration properties

| Property | Description | Default |
| -------- | ----------- | :-----: |
| `includeOptionsRequests` | Determines whether to include `OPTIONS` requests in the generated OpenAPI spec | `false` |
| `ignoreResponseTypes` | Determines whether to ignore response types in the generated OpenAPI spec. | `false` |
| `specVersion` | Determines which version to use for the generated OpenAPI spec. Can be set to `v2_0` or `v3_0` | `v3_0` |
| `specFormat` | Determines which format to use for the generated OpenAPI spec. Can be set to `Json` or `Yaml` | `Json` |
| `includeParameters` | Determines the list of query string parameters to include in the generated OpenAPI spec, along with their default values. | [] |

## Command line options

None

## Remarks

To create better OpenAPI specs, consider using a local language model with Dev Proxy. Using a local language model, the `OpenApiSpecGeneratorPlugin` generates clearer operation IDs and descriptions, giving you a better starting point for your OpenAPI spec. To use a local language model with the `OpenApiSpecGeneratorPlugin`, enable the language model in the configuration file. For more information, see [Use a local language model](../how-to/use-language-model.md).

## Next step

> [!div class="nextstepaction"]
> [Generate an OpenAPI spec](../how-to/generate-openapi-spec.md)
