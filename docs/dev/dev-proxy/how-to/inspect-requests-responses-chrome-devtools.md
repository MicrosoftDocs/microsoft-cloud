---
title: Inspect requests and responses using Chrome DevTools
description: How to use Chrome DevTools to inspect requests and responses intercepted by Dev Proxy
author: waldekmastykarz
ms.author: wmastyka
ms.date: 04/30/2025
---

# Inspect requests and responses using Chrome DevTools

By default, Dev Proxy displays its messages in the command prompt. If you use Dev Proxy with an application that issues many requests, it's difficult to find the messages you're interested in. What's more, you might want to inspect the requests and responses intercepted by Dev Proxy.

To make it easier to find the messages you're interested in, use the [`DevToolsPlugin`](../technical-reference/devtoolsplugin.md) plugin to display Dev Proxy messages in Chrome DevTools.

> [!TIP]
> Dev Proxy supports using Chrome DevTools with Microsoft Edge, Microsoft Edge Dev and Google Chrome.

The `DevToolsPlugin` exposes Dev Proxy messages, and information about intercepted requests and responses in Chrome DevTools.

:::image type="content" source="../media/devtools-messages.png" alt-text="Screenshot of Microsoft Edge with dev tools showing Dev Proxy messages." lightbox="../media/devtools-messages.png":::

:::image type="content" source="../media/devtools-requests.png" alt-text="Screenshot of Microsoft Edge with dev tools showing requests and responses intercepted by Dev Proxy." lightbox="../media/devtools-requests.png":::

To use Chrome DevTools with Dev Proxy:

1. Open the [devproxyrc.json](../technical-reference/devproxyrc.md) file stored in your Dev Proxy installation directory. You can also use the `devproxy config open` command.
1. Enable the `DevToolsPlugin` plugin, by adding the following fragment to the `plugins` array:

    ```json
    {
      "name": "DevToolsPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "devTools"
    }
    ```

1. Add the `devTools` section and specify your preferred browser:

    ```json
    "devTools": {
      "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v1.0.0/devtoolsplugin.schema.json",
      "preferredBrowser": "Edge"
    }
    ```

    > [!TIP]
    > Supported values are: `Edge`, `EdgeDev`, `Chrome`

1. The complete `devproxyrc.json` file looks like:

    ```json
    {
      "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v1.0.0/rc.schema.json",
      "plugins": [
        {
          "name": "DevToolsPlugin",
          "enabled": true,
          "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
          "configSection": "devTools"
        }
      ],
      "devTools": {
        "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v1.0.0/devtoolsplugin.schema.json",
        "preferredBrowser": "Edge"
      }
    }
    ```

1. Save the `devproxyrc.json` file and start Dev Proxy.
