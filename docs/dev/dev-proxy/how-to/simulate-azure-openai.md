---
title: Simulate Azure OpenAI API
description: How to simulate Azure OpenAI API
author: waldekmastykarz
ms.author: wmastyka
ms.date: 06/18/2024
---

# Simulate Azure OpenAI API

When you build apps connected to Azure OpenAI, often, only a portion of the app interacts with the Azure OpenAI API. When you work on the portions of the app that don't require real replies from Azure OpenAI API, you can simulate the responses using Dev Proxy. Using simulated responses allows you to avoid incurring unnecessary costs. The `OpenAIMockResponsePlugin` uses a local language model running on Ollama to simulate responses from Azure OpenAI API.

## Before you start

To simulate Azure OpenAI API responses using Dev Proxy, you need Ollama installed on your machine. To install Ollama, follow the instructions in the [Ollama documentation](https://github.com/ollama/ollama/blob/main/README.md).

By default, Dev Proxy uses the phi-3 language model. To use a different model, update the [`model` property](./use-language-model.md) in the Dev Proxy configuration file.

## Configure Dev Proxy to simulate Azure OpenAI API responses

> [!TIP]
> Steps described in this tutorial are available in a ready-to-use Dev Proxy preset. To use the preset, in command line, run `devproxy preset get simulate-azure-openai`, and follow the instructions.

To simulate Azure OpenAI API responses using Dev Proxy, you need to enable the `OpenAIMockResponsePlugin` in the `devproxyrc.json` file.

```json
{
  "$schema": "https://raw.githubusercontent.com/microsoft/dev-proxy/main/schemas/v0.19.0/rc.schema.json",
  "plugins": [
    {
      "name": "OpenAIMockResponsePlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/dev-proxy-plugins.dll"
    }
  ]
}
```

Next, configure Dev Proxy to intercept requests to Azure OpenAI API. For simplicity, use wildcards to intercept requests to all deployments.

```json
{
  // [...] trimmed for brevity
  "urlsToWatch": [
    "https://*.openai.azure.com/openai/deployments/*/completions*"
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
  "$schema": "https://raw.githubusercontent.com/microsoft/dev-proxy/main/schemas/v0.19.0/rc.schema.json",
  "plugins": [
    {
      "name": "OpenAIMockResponsePlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/dev-proxy-plugins.dll"
    }
  ],
  "urlsToWatch": [
    "https://*.openai.azure.com/openai/deployments/*/completions*"
  ],
  "languageModel": {
    "enabled": true
  }
}
```

## Simulate Azure OpenAI API responses

Start Ollama with the phi-3 language model. In the command line, run `ollama run phi3`.

Next, start Dev Proxy. If you use the preset, run `devproxy -c "~appFolder/presets/simulate-azure-openai/simulate-azure-openai.json`. If you use a custom configuration file named `devproxyrc.json`, stored in the current working directory, run `devproxy`. Dev Proxy checks that it can access the Ollama language model and confirms that it's ready to simulate Azure OpenAI API responses.

```text
 info    OpenAIMockResponsePlugin: Checking language model availability...
 info    Listening on 127.0.0.1:8000...

Hotkeys: issue (w)eb request, (r)ecord, (s)top recording, (c)lear screen
Press CTRL+C to stop Dev Proxy
```

Run your application and make requests to the Azure OpenAI API. Dev Proxy intercepts the requests and simulates responses using the local language model.

:::image type="content" source="../media/openai-mock-response-plugin.png" alt-text="Screenshot of a command prompt with Dev Proxy simulating response for a request to Azure OpenAI API." lightbox="../media/openai-mock-response-plugin.png":::

## Next step

Learn more about the OpenAIMockResponsePlugin.

> [!div class="nextstepaction"]
> [OpenAIMockResponsePlugin](../technical-reference/openaimockresponseplugin.md)

## Samples

See also the related Dev Proxy samples:

- [Simulate Azure OpenAI API](https://adoption.microsoft.com/sample-solution-gallery/sample/pnp-devproxy-simulate-azure-openai/)
