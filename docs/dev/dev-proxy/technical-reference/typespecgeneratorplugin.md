---
title: TypeSpecGeneratorPlugin
description: TypeSpecGeneratorPlugin reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/06/2026
---

<!-- INTENT: Generate TypeSpec files from intercepted API traffic -->
<!-- PLUGIN-TYPE: Reporting -->
<!-- WORKS-WITH: OpenApiSpecGeneratorPlugin, MockGeneratorPlugin -->
<!-- USE-WHEN: Creating TypeSpec definitions from existing APIs -->

# TypeSpecGeneratorPlugin

Generates TypeSpec files from the intercepted requests and responses.

:::image type="content" source="../media/type-spec-generator-plugin.png" alt-text="Screenshot of two command prompt windows. One shows Dev Proxy recording API requests. The other shows the generated TypeSpec file." lightbox="../media/type-spec-generator-plugin.png":::

## Configuration example

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.0.0/rc.schema.json",
  "plugins": [
    {
      "name": "TypeSpecGeneratorPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "typeSpecGeneratorPlugin"
    }
  ],
  "typeSpecGeneratorPlugin": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.0.0/typespecgeneratorplugin.schema.json",
    "ignoreResponseTypes": false
  }
}
```

## Configuration properties

| Property | Description | Default |
| -------- | ----------- | :-----: |
| `ignoreResponseTypes` | Determines whether to generate types for API responses (`false`) or to set them to `string` (`true`). | `false` |

## Command line options

None

## Remarks

To create better TypeSpec files, consider using a local language model with Dev Proxy. Using a local language model, the `TypeSpecGeneratorPlugin` generates clearer operation IDs and descriptions, giving you a better starting point for your TypeSpec file. To use a local language model with the `TypeSpecGeneratorPlugin`, enable the language model in the configuration file. For more information, see [Use a local language model](../how-to/use-language-model.md).
