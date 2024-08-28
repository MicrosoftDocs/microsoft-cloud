---
title: Test that my application handles throttling properly
description: How to test that your application handles throttling properly
author: waldekmastykarz
ms.author: wmastyka
ms.date: 08/26/2024
---

# Test that my application handles throttling properly

Testing throttling is difficult because it occurs rarely, only when the server hosting the API is under heavy load. Using the Dev Proxy, you can simulate throttling on any API, and check if your application handles it correctly.

To simulate throttling on any API, use the [GenericRandomErrorPlugin](../technical-reference/genericrandomerrorplugin.md). If the API that you use, returns a `Retry-After` header, use the [RetryAfterPlugin](../technical-reference/retryafterplugin.md) to verify that your app backs-off as instructed by the API.

## Simulate throttling on any API

To start, enable the `GenericRandomErrorPlugin` in your Dev Proxy configuration file.

```json
{
  "$schema": "https://raw.githubusercontent.com/microsoft/dev-proxy/main/schemas/v0.20.0/rc.schema.json",
  "plugins": [
    {
      "name": "GenericRandomErrorPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/dev-proxy-plugins.dll",
      "configSection": "errorsContosoApi",
      "urlsToWatch": [
        "https://api.contoso.com/*"
      ]
    }
  ]
}
```

Next, configure the plugin to use a file that contains the errors you want to simulate.

```json
{
  "$schema": "https://raw.githubusercontent.com/microsoft/dev-proxy/main/schemas/v0.20.0/rc.schema.json",
  "plugins": [
    {
      "name": "GenericRandomErrorPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/dev-proxy-plugins.dll",
      "configSection": "errorsContosoApi",
      "urlsToWatch": [
        "https://api.contoso.com/*"
      ]
    }
  ],
  "errorsContosoApi": {
    "errorsFile": "errors-contoso-api.json"
  }
}
```

In the errors file, define the throttling response so that it matches the actual throttling response of your API:

```json
{
  "$schema": "https://raw.githubusercontent.com/microsoft/dev-proxy/main/schemas/v0.20.0/genericrandomerrorplugin.schema.json",
  "errors": [
    {
      "request": {
        "url": "https://api.contoso.com/*"
      },
      "responses": [
        {
          "statusCode": 429,
          "headers": [
            {
              "name": "Content-Type",
              "value": "application/json"
            }
          ],
          "body": {
            "code": "TooManyRequests",
            "message": "Too many requests"
          }
        }
      ]
    }
  ]
}
```

Start Dev Proxy with your configuration file and test your app to see how it handles throttling.

## Test correct backing off with the `Retry-After` header

Many APIs use the `Retry-After` response header to instruct the app to back-off for a specific amount of time. When simulating throttling responses using Dev Proxy, you can either configure the `Retry-After` header to a static value, or use a dynamic value that tests if your app is waiting as instructed before calling the API again.

To configure the `Retry-After` header to a static value, add the header to your throttling response:

```json
{
  "$schema": "https://raw.githubusercontent.com/microsoft/dev-proxy/main/schemas/v0.20.0/genericrandomerrorplugin.schema.json",
  "errors": [
    {
      "request": {
        "url": "https://api.contoso.com/*"
      },
      "responses": [
        {
          "statusCode": 429,
          "headers": [
            {
              "name": "Content-Type",
              "value": "application/json"
            },
            {
              "name": "Retry-After",
              "value": "60"
            }
          ],
          "body": {
            "code": "TooManyRequests",
            "message": "Too many requests"
          }
        }
      ]
    }
  ]
}
```

In this example, the `Retry-After` header is set to 60 seconds. When you configure the header to a static value, Dev Proxy isn't controlling if your app is waiting before calling the API again.

To test if your app is correctly waiting before calling the API again, change the header's value to `@dynamic`:

```json
{
  "$schema": "https://raw.githubusercontent.com/microsoft/dev-proxy/main/schemas/v0.20.0/genericrandomerrorplugin.schema.json",
  "errors": [
    {
      "request": {
        "url": "https://api.contoso.com/*"
      },
      "responses": [
        {
          "statusCode": 429,
          "headers": [
            {
              "name": "Content-Type",
              "value": "application/json"
            },
            {
              "name": "Retry-After",
              "value": "@dynamic"
            }
          ],
          "body": {
            "code": "TooManyRequests",
            "message": "Too many requests"
          }
        }
      ]
    }
  ]
}
```

Additionally, extend your Dev Proxy configuration with the [`RetryAfterPlugin`](../technical-reference/retryafterplugin.md).

```json
{
  "$schema": "https://raw.githubusercontent.com/microsoft/dev-proxy/main/schemas/v0.20.0/rc.schema.json",
  "plugins": [
    {
      "name": "RetryAfterPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/dev-proxy-plugins.dll",
      "urlsToWatch": [
        "https://api.contoso.com/*"
      ]
    },
    {
      "name": "GenericRandomErrorPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/dev-proxy-plugins.dll",
      "configSection": "errorsContosoApi",
      "urlsToWatch": [
        "https://api.contoso.com/*"
      ]
    }
  ],
  "errorsContosoApi": {
    "errorsFile": "errors-contoso-api.json"
  }
}
```

> [!CAUTION]
> Add the `RetryAfterPlugin` before the `GenericRandomErrorPlugin` in your configuration file. If you add it after, the request will be failed by the `GenericRandomErrorPlugin` before the `RetryAfterPlugin` has a chance to handle it.

This plugin keeps track of throttling responses and forcefully fails requests issued to APIs that are still throttled.

## More information

- [What is throttling?](../concepts/what-is-throttling.md)
- [How to handle API throttling](../concepts/how-to-handle-api-throttling.md)
