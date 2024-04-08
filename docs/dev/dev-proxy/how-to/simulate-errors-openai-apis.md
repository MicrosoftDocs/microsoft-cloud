---
title: Simulate errors from OpenAI APIs
description: How to configure Dev Proxy to simulate errors from OpenAI APIs
author: waldekmastykarz
ms.author: wmastyka
ms.date: 04/08/2024
---

# Simulate errors from OpenAI APIs

When you use OpenAI APIs in your app, you should test how your app handles API errors. Dev Proxy allows you to simulate errors on any OpenAI API using the [GenericRandomErrorPlugin](../technical-reference/genericrandomerrorplugin.md).

> [!TIP]
> Download this preset by running in the terminal `devproxy preset get openai-throttling`.

In the Dev Proxy install folder, locate the `presets` folder. In the `presets` folder, create a new file named `openai-errors.json`. Open the file in a code editor.

Create a new object in the `plugins` array referencing the `GenericRandomErrorPlugin`. Define the OpenAI API URL for the plugin to watch for and add a reference to the plugin configuration.

```json
{
  "$schema": "https://raw.githubusercontent.com/microsoft/dev-proxy/main/schemas/v0.14.1/rc.schema.json",
  "plugins": [    
    {
      "name": "GenericRandomErrorPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/dev-proxy-plugins.dll",
      "configSection": "openAIAPI",
      "urlsToWatch": [
        "https://api.openai.com/*"
      ]
    }
  ]
}
```

Create the plugin configuration object to provide the plugin with the location of the error responses.

```json
{
  "$schema": "https://raw.githubusercontent.com/microsoft/dev-proxy/main/schemas/v0.14.1/rc.schema.json",
  "plugins": [    
    {
      "name": "GenericRandomErrorPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/dev-proxy-plugins.dll",
      "configSection": "openAIAPI",
      "urlsToWatch": [
        "https://api.openai.com/*"
      ]
    }
  ],
  "openAIAPI": {
    "errorsFile": "errors-openai.json"
  }
}
```

In the same folder, create the `errors-openai.json` file. This file contains the possible error responses that can be returned when the plugin sends an error response.

```json
{
  "responses": [
    {
      "statusCode": 429,
      "headers": [
        {
          "name": "content-type",
          "value": "application/json; charset=utf-8"
        }
      ],
      "body": {
        "error": {
          "message": "Rate limit reached for default-text-davinci-003 in organization org-K7hT684bLccDbBRnySOoK9f2 on tokens per min. Limit: 150000.000000 / min. Current: 160000.000000 / min. Contact support@openai.com if you continue to have issues. Please add a payment method to your account to increase your rate limit. Visit https://beta.openai.com/account/billing to add a payment method.",
          "type": "tokens",
          "param": null,
          "code": null
        }
      }
    },
    {
      "statusCode": 429,
      "headers": [
        {
          "name": "content-type",
          "value": "application/json; charset=utf-8"
        }
      ],
      "body": {
        "error": {
          "message": "Rate limit reached for default-text-davinci-003 in organization org-K7hT684bLccDbBRnySOoK9f2 on requests per min. Limit: 60.000000 / min. Current: 70.000000 / min. Contact support@openai.com if you continue to have issues. Please add a payment method to your account to increase your rate limit. Visit https://beta.openai.com/account/billing to add a payment method.",
          "type": "requests",
          "param": null,
          "code": null
        }
      }
    },
    {
      "statusCode": 429,
      "addDynamicRetryAfter": true,
      "headers": [
        {
          "name": "content-type",
          "value": "application/json; charset=utf-8"
        }
      ],
      "body": {
        "error": {
          "message": "The engine is currently overloaded, please try again later.",
          "type": "requests",
          "param": null,
          "code": null
        }
      }
    }
  ]
}
```

Start Dev Proxy with the configuration file:

```console
devproxy --config-file "~appFolder/presets/openai-errors.json"
```

When you use your app calling OpenAI APIs, Dev Proxy randomly returns one of the error responses you defined in the `errors-openai.json` file.

Learn more about the GenericRandomErrorPlugin.

> [!div class="nextstepaction"]
> [GenericRandomErrorPlugin](../technical-reference/genericrandomerrorplugin.md)
