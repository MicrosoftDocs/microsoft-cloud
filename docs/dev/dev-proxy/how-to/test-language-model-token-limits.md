---
title: Test language model token limits
description: How to test your app with language model token rate limiting
author: waldekmastykarz
ms.author: wmastyka
ms.date: 07/21/2025
---

# Test language model token limits

When building apps that use language models, you should test how your app handles token-based rate limiting. Dev Proxy allows you to simulate token limits on language model APIs using the [LanguageModelRateLimitingPlugin](../technical-reference/languagemodelratelimitingplugin.md).

## Simulate token limits on language model APIs

To start, enable the `LanguageModelRateLimitingPlugin` in your configuration file.

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v1.0.0/rc.schema.json",
  "plugins": [
    {
      "name": "LanguageModelRateLimitingPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "languageModelRateLimitingPlugin",
      "urlsToWatch": [
        "https://api.openai.com/*",
        "http://localhost:11434/*"
      ]
    }
  ]
}
```

> [!TIP]
> The plugin works with any OpenAI-compatible API, including local language models like Ollama. Include all the language model API endpoints you want to test in the `urlsToWatch` property.

Next, configure the plugin with your desired token limits and time window.

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v1.0.0/rc.schema.json",
  "plugins": [
    {
      "name": "LanguageModelRateLimitingPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "languageModelRateLimitingPlugin",
      "urlsToWatch": [
        "https://api.openai.com/*",
        "http://localhost:11434/*"
      ]
    }
  ],
  "languageModelRateLimitingPlugin": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v1.0.0/languagemodelratelimitingplugin.schema.json",
    "promptTokenLimit": 1000,
    "completionTokenLimit": 500,
    "resetTimeWindowSeconds": 60,
    "whenLimitExceeded": "Throttle"
  }
}
```

This configuration allows up to 1,000 prompt tokens and 500 completion tokens within a 60-second window. When either limit is exceeded, subsequent requests are throttled with a standard 429 response.

Start Dev Proxy with your configuration file and use your app to make language model requests. The plugin tracks token consumption from actual API responses and throttles requests when limits are exceeded.

## Test with custom error responses

You can also configure custom responses when token limits are exceeded by setting `whenLimitExceeded` to `Custom` and creating a custom response file.

```json
{
  "languageModelRateLimitingPlugin": {
    "promptTokenLimit": 500,
    "completionTokenLimit": 300,
    "resetTimeWindowSeconds": 120,
    "whenLimitExceeded": "Custom",
    "customResponseFile": "token-limit-response.json"
  }
}
```

Create the custom response file with your desired error format:

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v1.0.0/languagemodelratelimitingplugincustomresponse.schema.json",
  "statusCode": 429,
  "headers": [
    {
      "name": "retry-after",
      "value": "@dynamic"
    },
    {
      "name": "content-type",
      "value": "application/json"
    }
  ],
  "body": {
    "error": {
      "message": "Token quota exceeded. Your application has consumed all available tokens for this time period.",
      "type": "quota_exceeded",
      "code": "TOKENS_EXHAUSTED",
      "details": {
        "quota_type": "tokens",
        "retry_after_seconds": "@dynamic"
      }
    }
  }
}
```

The `@dynamic` value for retry-after headers automatically calculates the seconds remaining until the token limits reset.

## Test different scenarios

### Scenario 1: Low token limits for frequent testing

```json
{
  "languageModelRateLimitingPlugin": {
    "promptTokenLimit": 100,
    "completionTokenLimit": 50,
    "resetTimeWindowSeconds": 30
  }
}
```

Use low limits with short time windows to quickly trigger throttling during development and testing.

### Scenario 2: Production-like limits

```json
{
  "languageModelRateLimitingPlugin": {
    "promptTokenLimit": 10000,
    "completionTokenLimit": 5000,
    "resetTimeWindowSeconds": 3600
  }
}
```

To test realistic token consumption patterns, configure limits similar to your production environment.

### Scenario 3: Asymmetric limits

```json
{
  "languageModelRateLimitingPlugin": {
    "promptTokenLimit": 2000,
    "completionTokenLimit": 100,
    "resetTimeWindowSeconds": 300
  }
}
```

Test scenarios where completion token limits are lower than prompt limits, simulating cost-conscious API plans.

## Next step

Learn more about the `LanguageModelRateLimitingPlugin`.

> [!div class="nextstepaction"]
> [LanguageModelRateLimitingPlugin](../technical-reference/languagemodelratelimitingplugin.md)
