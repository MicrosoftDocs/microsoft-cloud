---
title: Use local language model with Dev Proxy
description: How to configure Dev Proxy to use a local language model
author: waldekmastykarz
ms.author: wmastyka
ms.date: 04/15/2025
---

# Use local language model with Dev Proxy

By connecting Dev Proxy to a local language model, you can improve Dev Proxy's functionality. Select Dev Proxy plugins use the language model to improve their output, where natural language is involved. By using a local language model, you can benefit from the improved functionality without incurring extra costs.

## Prerequisites

Dev Proxy supports Ollama and LM Studio as the client for local language models. To set them up on your machine, follow the instructions in their documentation.

## Configure Dev Proxy to use a local language model

To configure Dev Proxy to use a local language model, use the `languageModel` setting in the `devproxyrc.json` file.

```json
{
  "languageModel": {
    "enabled": true
  }
}
```

You can use the following options as part of the `languageModel` setting:

| Option | Description | Default value |
| --- | --- | :---: |
| `cacheResponses` | Specifies whether to cache responses from the language model. | `true` |
| `client` | The name of the client to use. Allowed values: `LMStudio`, `Ollama` | `Ollama` |
| `enabled` | Specifies whether to use a local language model. | `false` |
| `model` | The name of the language model to use. | `llama3.2` |
| `url` | The URL of the local language model client. | `http://localhost:11434` |

By default, Dev Proxy uses the standard Ollama configuration with the [llama3.2](https://ollama.com/library/llama3.2) model. It also caches responses from the language model, which means, that for the same prompt, you get an immediate response without waiting for the language model to process it.

> [!IMPORTANT]
> When using a local language model, be sure to start your local language model client before starting Dev Proxy.

## Dev Proxy plugins that can use a local language model

The following Dev Proxy plugins use a local language model if available:

- [OpenApiSpecGeneratorPlugin](../technical-reference/openapispecgeneratorplugin.md)
