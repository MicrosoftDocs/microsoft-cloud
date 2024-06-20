---
title: HttpFileGeneratorPlugin
description: HttpFileGeneratorPlugin reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 06/20/2024
---

# HttpFileGeneratorPlugin

Generates HTTP file from the intercepted requests and responses.

:::image type="content" source="../media/http-file-generator-plugin.png" alt-text="Screenshot of two command prompt windows. One shows Dev Proxy recording API requests. The other shows the generated HTTP file." lightbox="../media/http-file-generator-plugin.png":::

## Plugin instance definition

```json
{
  "name": "HttpFileGeneratorPlugin",
  "enabled": true,
  "pluginPath": "~appFolder/plugins/dev-proxy-plugins.dll",
  "configSection": "httpFileGeneratorPlugin"
}
```

## Configuration example

```json
{
  "httpFileGeneratorPlugin": {
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
Authorization: {{jsonplaceholder_typicode_com_authorization}}
Via: 1.1 dev-proxy/0.19.0
```

The plugin creates variables for each combination of hostname and request header/query string parameter. If multiple requests use the same combination, the plugin reuses the variable.
