---
title: TypeSpecGeneratorPlugin
description: TypeSpecGeneratorPlugin reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 11/18/2025
---

# TypeSpecGeneratorPlugin

Generates TypeSpec files from the intercepted requests and responses.

:::image type="content" source="../media/type-spec-generator-plugin.png" alt-text="Screenshot of two command prompt windows. One shows Dev Proxy recording API requests. The other shows the generated TypeSpec file." lightbox="../media/type-spec-generator-plugin.png":::

## Plugin instance definition

```json
{
  "name": "TypeSpecGeneratorPlugin",
  "enabled": true,
  "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
  "configSection": "typeSpecGeneratorPlugin"
}
```

## Configuration example

```json
{
  "typeSpecGeneratorPlugin": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v1.0.0/typespecgeneratorplugin.schema.json",
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
