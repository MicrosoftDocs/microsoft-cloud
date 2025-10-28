---
title: OpenAIUsageDebuggingPlugin
description: OpenAIUsageDebuggingPlugin reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 10/28/2025
---

# OpenAIUsageDebuggingPlugin

Logs OpenAI API usage metrics to a CSV file for debugging and analysis purposes.

## Plugin instance definition

```json
{
  "name": "OpenAIUsageDebuggingPlugin",
  "enabled": true,
  "pluginPath": "~appFolder/plugins/dev-proxy-plugins.dll"
}
```

## Configuration example

None

## Configuration properties

None

## Command line options

None

## Remarks

The OpenAIUsageDebuggingPlugin captures detailed usage metrics from OpenAI-compatible API requests and responses and writes them to a CSV file. This information is useful for debugging, tracking token consumption, monitoring rate limits, and analyzing API usage patterns over time.

### Output file

The plugin creates a CSV file named `devproxy_llmusage_<timestamp>.csv` in the current directory when Dev Proxy starts. The timestamp format is `yyyyMMddHHmmss`.

### CSV file structure

The CSV file contains the following columns:

| Column | Description |
| ------ | ----------- |
| `time` | ISO 8601 timestamp of the request |
| `status` | HTTP status code of the response |
| `retry-after` | Value of the `retry-after` header (for rate-limited requests) |
| `policy` | Value of the `policy-id` header (for rate-limited requests) |
| `prompt tokens` | Number of tokens in the prompt/input |
| `completion tokens` | Number of tokens in the completion/output |
| `cached tokens` | Number of cached tokens (from prompt cache) |
| `total tokens` | Total number of tokens used (prompt + completion) |
| `remaining tokens` | Remaining tokens in the rate limit window |
| `remaining requests` | Remaining requests in the rate limit window |

### Sample output

```csv
time,status,retry-after,policy,prompt tokens,completion tokens,cached tokens,total tokens,remaining tokens,remaining requests
2025-10-28T10:15:30.123Z,200,,,150,75,,225,9850,49
2025-10-28T10:15:35.456Z,200,,,200,100,50,300,9550,48
2025-10-28T10:15:40.789Z,429,60,rate-limit-policy-1,,,,,,0
```

### Supported scenarios

The plugin logs metrics for:

- **Successful requests** (2xx status codes): Captures token usage metrics including prompt tokens, completion tokens, cached tokens, and remaining rate limits
- **Error responses** (4xx status codes): Captures rate limiting information including retry-after headers and policy IDs

### Streaming responses

The plugin correctly handles streaming responses (using `text/event-stream` content type) by extracting the final chunk containing usage information.

### Use cases

This plugin is useful for:

- **Debugging token consumption**: Understanding how many tokens your prompts and completions consume
- **Monitoring rate limits**: Tracking remaining tokens and requests to avoid hitting rate limits
- **Cost analysis**: Analyzing token usage patterns to estimate costs
- **Performance optimization**: Identifying requests with high token counts
- **Prompt caching analysis**: Tracking cached token usage to optimize prompt caching strategies

### Comparison with OpenAITelemetryPlugin

While the [OpenAITelemetryPlugin](openaitelemetryplugin.md) sends telemetry data to OpenTelemetry-compatible dashboards for real-time monitoring and visualization, the OpenAIUsageDebuggingPlugin focuses on creating detailed CSV logs for offline analysis and debugging. The two plugins complement each other:

- Use **OpenAITelemetryPlugin** for real-time monitoring, cost tracking, and integration with observability platforms
- Use **OpenAIUsageDebuggingPlugin** for detailed debugging, CSV-based analysis, and tracking rate limit information

You can enable both plugins simultaneously to get both real-time telemetry and detailed CSV logs.

## Next step

> [!div class="nextstepaction"]
> [Understand language model usage](../how-to/understand-language-model-usage.md)
