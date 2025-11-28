---
title: Generate a TypeSpec file
description: How to generate a TypeSpec from the intercepted API requests and responses
author: waldekmastykarz
ms.author: wmastyka
ms.date: 11/18/2025
---

# Generate a TypeSpec file

Dev Proxy allows you to generate a TypeSpec file from the intercepted API requests and responses. Using Dev Proxy you can quickly create a TypeSpec file for an existing API and benefit from the tooling that supports TypeSpec.

To generate a TypeSpec file using Dev Proxy:

1. In the configuration file, enable the [`TypeSpecGeneratorPlugin`](../technical-reference/typespecgeneratorplugin.md) plugin:

    ```json
    {
      "plugins": [
        {
          "name": "TypeSpecGeneratorPlugin",
          "enabled": true,
          "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll"
        }
      ]
      // [...] shortened for brevity
    }
    ```

1. Optionally, configure the plugin in the configuration file:

    ```json
    {
      "typeSpecGeneratorPlugin": {
        "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v1.0.0/typespecgeneratorplugin.schema.json",
        "ignoreResponseTypes": false
      }
      // [...] shortened for brevity
    }
    ```

1. In the configuration file, to the list of URLs to watch, add the URL of the API for which you want to generate a TypeSpec file:

    ```json
    { 
      "urlsToWatch": [
        "https://api.example.com/*",
      ]
      // [...] shortened for brevity
    }
    ```

    > [!TIP]
    > To create better TypeSpec files, consider using a local language model with Dev Proxy. Using a local language model, the TypeSpecGeneratorPlugin generates clearer operation IDs and descriptions, giving you a better starting point for your TypeSpec file. For more information, see [Use a local language model](./use-language-model.md).

1. Start Dev Proxy:

    ```console
    devproxy
    ```

1. Start recording requests by pressing `r`
1. Perform the requests you want to include in the TypeSpec file
1. Stop recording requests by pressing `s`
1. Dev Proxy generates a TypeSpec file and saves it to a file in the current directory. Dev Proxy names the file after the host name of the API followed by the current date and time, for example: `api.example.com-20231219091700.tsp`.

:::image type="content" source="../media/type-spec-generator-plugin.png" alt-text="Screenshot of two command prompt windows. One shows Dev Proxy recording API requests. The other shows the generated TypeSpec file." lightbox="../media/type-spec-generator-plugin.png":::

## Next steps

Learn more about the TypeSpecGeneratorPlugin.

> [!div class="nextstepaction"]
> [TypeSpecGeneratorPlugin](../technical-reference/typespecgeneratorplugin.md)
