---
title: Simulate errors from non-Microsoft 365 APIs
description: How to configure the proxy to simulate errors from non-Microsoft 365 APIs
author: garrytrinder
ms.author: garrytrinder
ms.contributors: garrytrinder
ms.date: 11/03/2023
ms.topic: conceptual
ms.service: microsoft-cloud-for-developers

categories:
  - developer-tools
products:
  - microsoft-365
  - microsoft-graph
  - sharepoint-online
  - m365
ms.custom:
  - fcp
  - team=cloud_advocates
---

# Simulate errors from non-Microsoft 365 APIs

By default, the proxy simulates error responses based on Microsoft Graph presets.

Whilst you can use the built-in chaos handler with any API, it returns error responses that simulate the Microsoft Graph API.

When using the proxy to test non-Microsoft 365 APIs, you should provide your own error responses.

The below steps show how to configure error responses for the Open AI API as an example.

Open `devproxyrc.json` in the install folder.

Create a new object in the `plugins` array referencing the `GenericRandomErrorPlugin`. Defines the specific URLs for the plugin to watch for and a reference to the plugin configuration.

```json
{
  "name": "GenericRandomErrorPlugin",
  "enabled": true,
  "pluginPath": "plugins\\dev-proxy-plugins.dll",
  "configSection": "openAIAPI",
  "urlsToWatch": [
    "https://api.openai.com/*"
  ]
}
```

Create the plugin configuration object in the root to provide the plugin with the failure rate and the location of the error responses.

```json
"openAIAPI": {
  "rate": 90,
  "errorsFile": "errors-openai.json"
}
```

Create the `errors-openai.json` file in the install folder. This file contains the possible error responses that can be returned when the plugin sends an error response.

```json
{
  "responses": [
    {
      "statusCode": 429,
      "headers": {
        "content-type": "application/json; charset=utf-8"
      },
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
      "headers": {
        "content-type": "application/json; charset=utf-8"
      },
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
      "headers": {
        "content-type": "application/json; charset=utf-8"
      },
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

If you added API URL to the global `urlsToWatch` array, you should remove it to avoid plugin conflicts.

You can also return binary data in your error response. In the following example, the proxy returns an image in the body of the error.

```json
{
  "statusCode": 429,
  "headers": {
    "content-type": "image/jpeg",
    "Access-Control-Allow-Origin": "*"
  },
  "body": "@/path/to/http-cats/429.jpeg"
},
```

After making the changes to the `devproxyrc.json` file, you'll need to restart the proxy for the changes to take effect.

