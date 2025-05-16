---
title: OpenAITelemetryPlugin
description: OpenAITelemetryPlugin reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 05/19/2025
---

# OpenAITelemetryPlugin

Logs OpenAI telemetry data from the intercepted OpenAI-compatible requests and responses.

:::image type="content" source="../media/openai-telemetry-aspire.png" alt-text="Screenshot of the .NET Aspire dashboard showing OpenAI telemetry data" lightbox="../media/openai-telemetry-aspire.png":::

:::image type="content" source="../media/openai-telemetry-openlit.png" alt-text="Screenshot of the OpenLIT dashboard showing OpenAI telemetry data" lightbox="../media/openai-telemetry-openlit.png":::

## Plugin instance definition

```json
{
  "name": "OpenAITelemetryPlugin",
  "enabled": true,
  "pluginPath": "~appFolder/plugins/dev-proxy-plugins.dll",
  "configSection": "openAITelemetryPlugin"
}
```

## Configuration example

```json
{
  "openAITelemetryPlugin": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v0.28.0/openaitelemetryplugin.schema.json",
    "application": "My app",
    "includeCosts": true,
    "pricesFile": "prices.json"
  }
}
```

## Configuration properties

| Property | Description | Default |
| -------- | ----------- | :-----: |
| `application` | Name of the application that issues the requests. Logged in the telemetry data to group usage by application. | `default` |
| `currency` | The currency in which prices are logged. Displayed in charts. | `USD` |
| `environment` | Environment in which the application is running. Logged in the telemetry data to group usage by environment. | `development` |
| `exporterEndpoint` | The URL of the OpenTelemetry endpoint to send the data to. Must be HTTP Protobuf endpoint. | `http://localhost:4318` |
| `includeCompletion` | Whether to include the completion in the telemetry data. | `true` |
| `includeCosts` | Whether to include the costs in the telemetry data. Requires to specify the prices file. | `false` |
| `includePrompt` | Whether to include the prompt in the telemetry data. | `true` |
| `pricesFile` | Path to the file with prices information. | `null` |

## Prices file example

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v0.28.0/openaitelemetryplugin.pricesfile.schema.json",
  "prices": {
    "gpt-3.5-turbo": {
      "input": 0.0015,
      "output": 0.002
    },
    "gpt-4": {
      "input": 0.03,
      "output": 0.06
    }
  }
}
```

## Prices file properties

| Property | Description | Default |
| -------- | ----------- | :-----: |
| `prices` | Prices for the models. The key is the model name, and the value is model prices object. | `{}` |

### Model prices object

Each model prices object has the following properties:

| Property | Description | Required | Default value | Sample value |
| -------- | ----------- | :------: | ------------- | ------------ |
| `input` | The price per million tokens for input/prompt tokens. | yes | `0.0` | `0.03` |
| `output` | The price per million tokens for output/completion tokens. | yes | `0.0` | `0.06` |

## Command line options

None

## Remarks

The OpenAITelemetryPlugin logs OpenTelemetry data from the OpenAI-compatible requests and responses that it intercepts. Without having to instrument your application with OpenTelemetry, you can quickly understand how your application uses large language models. Additionally, if you provide prices file for the models you use, you can also see the LLM-related costs that your application incurs.

For each intercepted request and response, the plugin logs a span. Additionally, it logs three metrics:

- `gen_ai.client.token.usage` - the number of tokens used in the request and response
- `gen_ai.usage.cost` - the cost of the tokens used in the request and response
- `gen_ai.usage.total_cost` - the total cost of all requests over the course of a session

> [!IMPORTANT]
> The cost metrics are only logged if you set the `includeCosts` property to `true` and provide the prices file. Otherwise, the plugin doesn't log the metrics.

To view the recorded telemetry data, you can use any OpenTelemetry-compatible dashboard. For example, you can use the [.NET Aspire dashboard](/dotnet/aspire/fundamentals/dashboard/standalone) or [OpenLIT](https://openlit.io/).

> [!IMPORTANT]
> To see the data, start the dashboard before you issue OpenAI-compatible request. Otherwise, there's no OpenTelemetry collector to receive the data that the plugin sends.

## Next step

> [!div class="nextstepaction"]
> [Understand language model usage](../how-to/understand-language-model-usage.md)
