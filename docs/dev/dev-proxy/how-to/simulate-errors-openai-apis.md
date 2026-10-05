---
title: Simulate errors from OpenAI APIs
description: How to configure Dev Proxy to simulate errors from OpenAI APIs
author: waldekmastykarz
ms.author: wmastyka
ms.date: 10/03/2026
---

<!-- INTENT: Test OpenAI API error handling -->
<!-- SOLUTION: Enable GenericRandomErrorPlugin with OpenAI error file and RetryAfterPlugin -->
<!-- RESULT: App receives simulated OpenAI errors -->
<!-- PLUGINS: GenericRandomErrorPlugin, RetryAfterPlugin -->
<!-- JOB: test-error-handling -->
<!-- TIME: 10 minutes -->

# Simulate errors from OpenAI APIs

> **At a glance**  
> **Goal:** Test OpenAI API error handling  
> **Time:** 10 minutes  
> **Plugins:** [GenericRandomErrorPlugin](../technical-reference/genericrandomerrorplugin.md), [RetryAfterPlugin](../technical-reference/retryafterplugin.md)  
> **Prerequisites:** [Set up Dev Proxy](../get-started/set-up.md)

When you use OpenAI APIs in your app, you should test how your app handles API errors. Dev Proxy allows you to simulate errors on any OpenAI API using the [GenericRandomErrorPlugin](../technical-reference/genericrandomerrorplugin.md). With the [RetryAfterPlugin](../technical-reference/retryafterplugin.md), Dev Proxy also checks that your app waits for the time in the `Retry-After` header before it calls the API again.

> [!TIP]
> Download this preset by running in the command prompt `devproxy config get openai-throttling`.

In your project folder, create a new file named `devproxyrc.json`. Open the file in a code editor.

Create a new object in the `plugins` array referencing the `GenericRandomErrorPlugin`. Define the OpenAI API URL for Dev Proxy to watch and add a reference to the plugin configuration.

**File:** devproxyrc.json

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v3.3.1/rc.schema.json",
  "plugins": [
    {
      "name": "GenericRandomErrorPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "openAIAPI"
    }
  ],
  "urlsToWatch": [
    "https://api.openai.com/*"
  ]
}
```

Add the `RetryAfterPlugin` and create the plugin configuration object to provide the `GenericRandomErrorPlugin` with the location of the error responses and the percentage of requests to fail.

**File:** devproxyrc.json (complete config)

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v3.3.1/rc.schema.json",
  "plugins": [
    {
      "name": "RetryAfterPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll"
    },
    {
      "name": "GenericRandomErrorPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "openAIAPI"
    }
  ],
  "urlsToWatch": [
    "https://api.openai.com/*"
  ],
  "openAIAPI": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v3.3.1/genericrandomerrorplugin.schema.json",
    "errorsFile": "openai-errors.json",
    "rate": 90
  }
}
```

> [!CAUTION]
> Add the `RetryAfterPlugin` before the `GenericRandomErrorPlugin` in your configuration file. If you add it after, the `GenericRandomErrorPlugin` fails the request before the `RetryAfterPlugin` can check it.

In the same folder, create the `openai-errors.json` file. This file contains the error responses that Dev Proxy chooses from when it fails a request. They match the errors that the OpenAI API returns:

| Status | `error.code` | What it simulates |
| ------ | ------------ | ----------------- |
| 429 | `rate_limit_exceeded` | Your app hit its tokens per minute (TPM) or requests per minute (RPM) limit. |
| 429 | `slow_down` | Your app's request rate increased too quickly. |
| 429 | `credit_balance_exhausted` | Your organization has no prepaid credits left. Retrying doesn't help. |
| 503 | `server_is_overloaded` | The model is temporarily overloaded. |

For more information about these errors, see [Error codes](https://developers.openai.com/api/docs/guides/error-codes) in the OpenAI documentation.

**File:** openai-errors.json

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v3.3.1/genericrandomerrorplugin.errorsfile.schema.json",
  "errors": [
    {
      "request": {
        "url": "https://api.openai.com/*"
      },
      "responses": [
        {
          "statusCode": 429,
          "headers": [
            {
              "name": "content-type",
              "value": "application/json; charset=utf-8"
            },
            {
              "name": "Retry-After",
              "value": "@dynamic"
            }
          ],
          "body": {
            "error": {
              "message": "Rate limit reached for gpt-4.1 in organization org-K7hT684bLccDbBRnySOoK9f2 on tokens per min (TPM): Limit 30000, Used 30000, Requested 1200. Please try again in 2.4s. Visit https://platform.openai.com/settings/organization/limits to learn more.",
              "type": "tokens",
              "param": null,
              "code": "rate_limit_exceeded"
            }
          }
        },
        {
          "statusCode": 429,
          "headers": [
            {
              "name": "content-type",
              "value": "application/json; charset=utf-8"
            },
            {
              "name": "Retry-After",
              "value": "@dynamic"
            }
          ],
          "body": {
            "error": {
              "message": "Rate limit reached for gpt-4.1 in organization org-K7hT684bLccDbBRnySOoK9f2 on requests per min (RPM): Limit 500, Used 500, Requested 1. Please try again in 120ms. Visit https://platform.openai.com/settings/organization/limits to learn more.",
              "type": "requests",
              "param": null,
              "code": "rate_limit_exceeded"
            }
          }
        },
        {
          "statusCode": 429,
          "headers": [
            {
              "name": "content-type",
              "value": "application/json; charset=utf-8"
            },
            {
              "name": "Retry-After",
              "value": "@dynamic"
            }
          ],
          "body": {
            "error": {
              "message": "Your request rate increased too quickly. Reduce your request rate and increase it gradually.",
              "type": "rate_limit_error",
              "param": null,
              "code": "slow_down"
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
              "message": "Your organization has no prepaid credits remaining. Add credits to continue using the API. For more information on this error, read the docs: https://developers.openai.com/api/docs/guides/error-codes.",
              "type": "insufficient_quota",
              "param": null,
              "code": "credit_balance_exhausted"
            }
          }
        },
        {
          "statusCode": 503,
          "headers": [
            {
              "name": "content-type",
              "value": "application/json; charset=utf-8"
            }
          ],
          "body": {
            "error": {
              "message": "The requested model is temporarily overloaded. Please try again later.",
              "type": "service_unavailable_error",
              "param": null,
              "code": "server_is_overloaded"
            }
          }
        }
      ]
    }
  ]
}
```

The `@dynamic` value sets the `Retry-After` header and tells the `RetryAfterPlugin` to track how long your app must wait. The `credit_balance_exhausted` response has no `Retry-After` header, because waiting doesn't fix it.

Start Dev Proxy in your project folder:

```console
devproxy
```

When your app calls OpenAI APIs, Dev Proxy fails 90% of the requests with a random error from the `openai-errors.json` file. If your app calls the API again before the time in the `Retry-After` header, the `RetryAfterPlugin` reports it and throttles the request.

Check that your app:

- Waits for the `Retry-After` time after a `rate_limit_exceeded` or `slow_down` error.
- Stops calling the API after a `credit_balance_exhausted` error, instead of retrying.
- Retries with a delay after a `server_is_overloaded` error, and shows a clear message when retries run out.

Learn more about the GenericRandomErrorPlugin.

> [!div class="nextstepaction"]
> [GenericRandomErrorPlugin](../technical-reference/genericrandomerrorplugin.md)

## See also

- [Test how your app handles OpenAI rate limits](./test-openai-rate-limit-handling.md)
- [Test my app with random errors](./test-my-app-with-random-errors.md)
- [Test language model token limits](./test-language-model-token-limits.md)
- [RetryAfterPlugin](../technical-reference/retryafterplugin.md)
- [Simulate OpenAI API](./simulate-openai.md)
- [Simulate Azure OpenAI API](./simulate-azure-openai.md)
