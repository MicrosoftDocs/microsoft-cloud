---
title: Generate an OpenAPI spec
description: How to generate an OpenAPI spec from the intercepted API requests and responses
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/03/2026
---

<!-- INTENT: Create an OpenAPI specification from intercepted API requests -->
<!-- SOLUTION: Enable OpenApiSpecGeneratorPlugin and record requests -->
<!-- RESULT: OpenAPI spec file documenting recorded API endpoints -->
<!-- PLUGINS: OpenApiSpecGeneratorPlugin -->
<!-- JOB: analyze-usage -->
<!-- TIME: 10 minutes -->

# Generate an OpenAPI spec

> **At a glance**  
> **Goal:** Create an OpenAPI specification from intercepted API requests  
> **Time:** 10 minutes  
> **Plugins:** [OpenApiSpecGeneratorPlugin](../technical-reference/openapispecgeneratorplugin.md)  
> **Prerequisites:** [Set up Dev Proxy](../get-started/set-up.md)

Dev Proxy allows you to generate an OpenAPI spec from the intercepted API requests and responses. Using Dev Proxy you can quickly create an OpenAPI spec for an existing API and benefit from the tooling that supports OpenAPI.

> [!VIDEO https://www.youtube.com/embed/qKiR_AS8LlE?si=VBtG3rmaSe5gYWd1&start=74]

To generate an OpenAPI spec using Dev Proxy:

1. In the configuration file, enable the [`OpenApiSpecGeneratorPlugin`](../technical-reference/openapispecgeneratorplugin.md) plugin:

    **File:** devproxyrc.json

    ```json
    {
      "plugins": [
        {
          "name": "OpenApiSpecGeneratorPlugin",
          "enabled": true,
          "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll"
        }
      ]
      // [...] shortened for brevity
    }
    ```

1. In the configuration file, to the list of URLs to watch, add the URL of the API for which you want to generate an OpenAPI spec:

    **File:** devproxyrc.json

    ```json
    { 
      "urlsToWatch": [
        "https://api.example.com/*",
      ]
      // [...] shortened for brevity
    }
    ```

    > [!TIP]
    > To create better OpenAPI specs, consider using a local language model with Dev Proxy. For more information, see [Use a local language model](./use-language-model.md).

1. Start Dev Proxy:

    ```console
    devproxy
    ```

1. Start recording requests by pressing `r`
1. Perform the requests you want to include in the OpenAPI spec
1. Stop recording requests by pressing `s`
1. Dev Proxy generates an OpenAPI spec and saves it to a file in the current directory. Dev Proxy names the file after the host name of the API followed by the current date and time, for example: `api.example.com-20231219091700.json`.

:::image type="content" source="../media/open-api-spec-generator-plugin.png" alt-text="Screenshot of two command prompt windows. One shows Dev Proxy recording API requests. The other shows the generated OpenAPI spec." lightbox="../media/open-api-spec-generator-plugin.png":::

## See also

- [OpenApiSpecGeneratorPlugin](../technical-reference/openapispecgeneratorplugin.md) - Full reference
- [Use local language model](./use-language-model.md) - Improve generated specs
- [What is an OpenAPI spec](../concepts/what-is-openapi-spec.md) - Concepts
- [Glossary](../concepts/glossary.md) - Dev Proxy terminology
