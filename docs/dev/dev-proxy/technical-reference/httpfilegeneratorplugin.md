---
title: HttpFileGeneratorPlugin
description: HttpFileGeneratorPlugin reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/06/2026
---

<!-- INTENT: Generate HTTP files from intercepted requests -->
<!-- PLUGIN-TYPE: Reporting -->
<!-- WORKS-WITH: OpenApiSpecGeneratorPlugin, MockGeneratorPlugin -->
<!-- USE-WHEN: Creating replayable API request collections -->

# HttpFileGeneratorPlugin

Generates HTTP file from the intercepted requests and responses.

:::image type="content" source="../media/http-file-generator-plugin.png" alt-text="Screenshot of two command prompt windows. One shows Dev Proxy recording API requests. The other shows the generated HTTP file." lightbox="../media/http-file-generator-plugin.png":::

## Configuration example

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/rc.schema.json",
  "plugins": [
    {
      "name": "HttpFileGeneratorPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "httpFileGeneratorPlugin"
    }
  ],
  "httpFileGeneratorPlugin": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/httpfilegeneratorplugin.schema.json",
    "includeOptionsRequests": false
  }
}
```

## Configuration properties

| Property | Description | Default |
| -------- | ----------- | :-----: |
| `includeOptionsRequests` | Determines whether to include `OPTIONS` requests in the generated HTTP file | `false` |

## Command line options

None

## Remarks

When the plugin generates the HTTP file, it extracts authorization information such as bearer tokens and API keys from request headers and query string parameters. It replaces the actual values with placeholders and stores them in variables for easier management.

For example, for the following request:

```text
GET https://jsonplaceholder.typicode.com/posts?api-key=123
```

The plugin generates the following HTTP file:

```http
@jsonplaceholder_typicode_com_api_key = api-key

###

# @name getPosts

GET https://jsonplaceholder.typicode.com/posts?api-key={{jsonplaceholder_typicode_com_api_key}}
Host: jsonplaceholder.typicode.com
User-Agent: curl/8.6.0
Accept: */*
Via: 1.1 dev-proxy/0.27.0
```

The plugin creates variables for each combination of hostname and request header/query string parameter. If multiple requests use the same combination, the plugin reuses the variable.
