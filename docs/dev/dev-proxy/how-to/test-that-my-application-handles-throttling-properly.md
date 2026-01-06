---
title: Test that my application handles throttling properly
description: How to test that your application handles throttling properly
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/06/2026
---

<!-- INTENT: Test throttling handling with Retry-After headers on any API -->
<!-- SOLUTION: Enable GenericRandomErrorPlugin with 429 + RetryAfterPlugin -->
<!-- RESULT: App receives throttling responses and handles retries -->
<!-- PLUGINS: GenericRandomErrorPlugin, RetryAfterPlugin -->
<!-- JOB: test-error-handling -->
<!-- TIME: 15 minutes -->

# Test that my application handles throttling properly

> **At a glance**  
> **Goal:** Test how your app handles API throttling on any API  
> **Time:** 15 minutes  
> **Plugins:** [GenericRandomErrorPlugin](../technical-reference/genericrandomerrorplugin.md), [RetryAfterPlugin](../technical-reference/retryafterplugin.md)  
> **Prerequisites:** [Set up Dev Proxy](../get-started/set-up.md)

Testing throttling is difficult because it occurs rarely, only when the server hosting the API is under heavy load. Using the Dev Proxy, you can simulate throttling on any API, and check if your application handles it correctly.

To simulate throttling on any API, use the [GenericRandomErrorPlugin](../technical-reference/genericrandomerrorplugin.md). If the API that you use, returns a `Retry-After` header, use the [RetryAfterPlugin](../technical-reference/retryafterplugin.md) to verify that your app backs-off as instructed by the API.

## Simulate throttling on any API

To start, enable the `GenericRandomErrorPlugin` in your Dev Proxy configuration file.

**File:** devproxyrc.json

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.0.0/rc.schema.json",
  "plugins": [
    {
      "name": "GenericRandomErrorPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "errorsContosoApi",
      "urlsToWatch": [
        "https://api.contoso.com/*"
      ]
    }
  ]
}
```

Next, configure the plugin to use a file that contains the errors you want to simulate.

**File:** devproxyrc.json (with errorsFile configuration)

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.0.0/rc.schema.json",
  "plugins": [
    {
      "name": "GenericRandomErrorPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
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

**File:** errors-contoso-api.json

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.0.0/genericrandomerrorplugin.errorsfile.schema.json",
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

**File:** errors-contoso-api.json (with static Retry-After)

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.0.0/genericrandomerrorplugin.errorsfile.schema.json",
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

**File:** errors-contoso-api.json (with dynamic Retry-After)

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.0.0/genericrandomerrorplugin.errorsfile.schema.json",
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

**File:** devproxyrc.json (complete with RetryAfterPlugin)

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.0.0/rc.schema.json",
  "plugins": [
    {
      "name": "RetryAfterPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "urlsToWatch": [
        "https://api.contoso.com/*"
      ]
    },
    {
      "name": "GenericRandomErrorPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
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

## See also

- [GenericRandomErrorPlugin](../technical-reference/genericrandomerrorplugin.md) - Full reference
- [RetryAfterPlugin](../technical-reference/retryafterplugin.md) - Verify retry behavior
- [Simulate throttling on Microsoft 365 APIs](./simulate-throttling-microsoft-365.md) - Microsoft 365 specific
- [Glossary](../concepts/glossary.md) - Dev Proxy terminology
