---
title: DevToolsPlugin
description: DevToolsPlugin reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 1/9/2024
---

# DevToolsPlugin

Exposes Dev Proxy messages, and information about intercepted requests and responses in Chrome DevTools.

:::image type="content" source="../media/devtools-messages.png" alt-text="Screenshot of Microsoft Edge with dev tools showing Dev Proxy messages." lightbox="../media/devtools-messages.png":::

:::image type="content" source="../media/devtools-requests.png" alt-text="Screenshot of Microsoft Edge with dev tools showing requests and responses intercepted by Dev Proxy." lightbox="../media/devtools-requests.png":::

## Plugin instance definition

```json
{
  "name": "DevToolsPlugin",
  "enabled": true,
  "pluginPath": "plugins\\dev-proxy-plugins.dll",
  "configSection": "devTools"
}
```

## Configuration example

```json
{
  "devTools": {
    "preferredBrowser": "Edge"
  }
}
```

## Configuration properties

Property | Description | Default
-------- | ----------- | :-----:
`preferredBrowser` | Which browser to use to launch Dev Tools. Supported values: `Edge`, `EdgeDev`, `Chrome` | `Edge` |

## Command line options

None

## Known issues

### Dev Tools don't open in Microsoft Edge on Windows

You use Dev Proxy on Windows and configure it to use Microsoft Edge to display Dev Tools. After you start Dev Proxy, it starts the inspector but Dev Tools don't open or they open empty.

To fix this issue:

1. Open Microsoft Edge
1. Go to **Settings**
1. Open **System and performance**
1. Disable **Startup boost**
1. Close all Microsoft Edge windows and processes
1. Start Dev Proxy
