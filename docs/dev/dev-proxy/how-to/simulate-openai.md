---
title: Simulate OpenAI API
description: How to simulate OpenAI API
author: waldekmastykarz
ms.author: wmastyka
ms.date: 07/14/2025
---

# Simulate OpenAI API

When you build apps connected to OpenAI, often, only a portion of the app interacts with the OpenAI API. When you work on the portions of the app that don't require real replies from OpenAI API, you can simulate the responses using Dev Proxy. Using simulated responses allows you to avoid incurring unnecessary costs. The `OpenAIMockResponsePlugin` uses a local language model to simulate responses from OpenAI API.

## Before you start

To simulate OpenAI API responses using Dev Proxy, you need a [supported language model client](./use-language-model.md) installed on your machine.

By default, Dev Proxy uses the llama3.2 language model running on Ollama. To use a different client or model, update the [language model settings](./use-language-model.md) in the Dev Proxy configuration file.

## Configure Dev Proxy to simulate OpenAI API responses

> [!TIP]
> Steps described in this tutorial are available in a ready-to-use Dev Proxy preset. To use the preset, in command line, run `devproxy config get simulate-openai`, and follow the instructions.

To simulate OpenAI API responses using Dev Proxy, you need to enable the `OpenAIMockResponsePlugin` in the `devproxyrc.json` file.

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v0.29.2/rc.schema.json",
  "plugins": [
    {
      "name": "OpenAIMockResponsePlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll"
    }
  ]
}
```

Next, configure Dev Proxy to intercept requests to OpenAI API. OpenAI recommends using the `https://api.openai.com/v1/chat/completions` endpoint, which allows you to benefit from the latest models and features.

```json
{
  // [...] trimmed for brevity
  "urlsToWatch": [
    "https://api.openai.com/v1/chat/completions"
  ]
}
```

Finally, configure Dev Proxy to use a local language model.

```json
{
  // [...] trimmed for brevity
  "languageModel": {
    "enabled": true
  }
}
```

The complete configuration file looks like this.

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v0.29.2/rc.schema.json",
  "plugins": [
    {
      "name": "OpenAIMockResponsePlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll"
    }
  ],
  "urlsToWatch": [
    "https://api.openai.com/v1/chat/completions"
  ],
  "languageModel": {
    "enabled": true
  }
}
```

## Simulate OpenAI API responses

Assuming the default configuration, start Ollama with the llama3.2 language model. In the command line, run `ollama run llama3.2`.

Next, start Dev Proxy. If you use the preset, run `devproxy -c "~appFolder/config/simulate-openai/simulate-openai.json`. If you use a custom configuration file named `devproxyrc.json`, stored in the current working directory, run `devproxy`. Dev Proxy checks that it can access the language model on Ollama and confirms that it's ready to simulate OpenAI API responses.

```text
 info    OpenAIMockResponsePlugin: Checking language model availability...
 info    Listening on 127.0.0.1:8000...

Hotkeys: issue (w)eb request, (r)ecord, (s)top recording, (c)lear screen
Press CTRL+C to stop Dev Proxy
```

Run your application and make requests to the OpenAI API. Dev Proxy intercepts the requests and simulates responses using the local language model.

:::image type="content" source="../media/openai-mock-response-plugin-openai-api.png" alt-text="Screenshot of a command prompt with Dev Proxy simulating response for a request to OpenAI API." lightbox="../media/openai-mock-response-plugin-openai-api.png":::

## Next step

Learn more about the OpenAIMockResponsePlugin.

> [!div class="nextstepaction"]
> [OpenAIMockResponsePlugin](../technical-reference/openaimockresponseplugin.md)

## Samples

See also the related Dev Proxy samples:

- [Simulate OpenAI API](https://adoption.microsoft.com/sample-solution-gallery/sample/pnp-devproxy-simulate-openai/)
