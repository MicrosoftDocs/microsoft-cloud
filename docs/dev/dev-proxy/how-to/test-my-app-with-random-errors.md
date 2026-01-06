---
title: Test my app with random errors
description: How to test your app with random errors
author: garrytrinder
ms.author: garrytrinder
ms.date: 01/06/2026
---

<!-- INTENT: Test how app handles API failures -->
<!-- SOLUTION: Enable GenericRandomErrorPlugin with error file -->
<!-- RESULT: App receives random HTTP errors at configured rate -->
<!-- PLUGINS: GenericRandomErrorPlugin -->
<!-- JOB: test-error-handling -->

# Test my app with random errors

> **At a glance**  
> **Goal:** See how your app handles API errors  
> **Time:** 5 minutes  
> **Plugins:** [GenericRandomErrorPlugin](../technical-reference/genericrandomerrorplugin.md)  
> **Prerequisites:** [Set up Dev Proxy](../get-started/set-up.md)

When building apps, you should test how your app handles API errors. Dev Proxy allows you to simulate errors on any API that you use in your app using the [GenericRandomErrorPlugin](../technical-reference/genericrandomerrorplugin.md).

## Simulate errors on any API

To start, enable the `GenericRandomErrorPlugin` in your configuration file.

**File:** `devproxyrc.json`

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

> [!TIP]
> Because each API is different, you typically configure an instance of the `GenericRandomErrorPlugin` for each API you want to simulate errors on. To make it easier to manage the configuration, name the `configSection` after the API you want to simulate errors on. Additionally, specify the URLs that you want to simulate errors on in the `urlsToWatch` property with the plugin. This will make it easier to manage the configuration and reuse it in the future.

Next, configure the plugin to use a file that contains the errors you want to simulate.

**File:** `devproxyrc.json` (add to same file)

```json
{
  "errorsContosoApi": {
    "errorsFile": "errors-contoso-api.json"
  }
}
```

Finally, in the errors file, define the list of error responses that you want to simulate. For example, to simulate a 500 error with a custom JSON response, use the following configuration:

**File:** `errors-contoso-api.json`

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
          "statusCode": 500,
          "headers": [
            {
              "name": "content-type",
              "value": "application/json; charset=utf-8"
            }
          ],
          "body": {
            "code": "InternalServerError",
            "message": "Something went wrong"
          }
        }
      ]
    }
  ]
}
```

You can define as many error responses as you need.

Start Dev Proxy with your configuration file and use your app to see how it handles the errors. For each matching request, Dev Proxy determines whether to simulate an error or pass the request through to the original API using the [configured failure rate](./change-request-failure-rate.md). When Dev Proxy simulates an error, it uses a random error from the array of error responses you defined in the configuration file.

## Temporarily disable mocks

If you use mocks in your configuration file, you can temporarily disable them by using the `--no-mocks` option.

```console
devproxy --no-mocks
```

## Next step

Learn more about the `GenericRandomErrorPlugin`.

> [!div class="nextstepaction"]
> [GenericRandomErrorPlugin](../technical-reference/genericrandomerrorplugin.md)

## See also

- [Simulate slow API responses](./Simulate-slow-API-responses.md) - Add latency to API calls
- [Simulate Rate-Limit API responses](./Simulate-Rate-Limit-API-responses.md) - Test rate limiting
- [Change request failure rate](./Change-request-failure-rate.md) - Adjust how often errors occur
- [Simulate errors from Microsoft Graph APIs](./simulate-errors-microsoft-graph-apis.md) - Microsoft Graph-specific errors
- [Glossary](../concepts/glossary.md) - Dev Proxy terminology
