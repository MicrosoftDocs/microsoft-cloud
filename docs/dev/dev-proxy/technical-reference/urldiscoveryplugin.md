---
title: UrlDiscoveryPlugin
description: UrlDiscoveryPlugin reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/24/2025
---

# UrlDiscoveryPlugin

Creates a list of URLs that Dev Proxy intercepted. Use this plugin to learn which URLs your app is calling and how to configure the Dev Proxy to simulate behaviors for them.

> [!TIP]
> To save the summary to a file, use a reporter plugin in your configuration.

:::image type="content" source="../media/url-discovery-plugin.png" alt-text="Screenshot of a split command prompt window. On the left side, Dev Proxy output showing intercepted requests. On the right side, generated report with intercepted URLs." lightbox="../media/url-discovery-plugin.png":::

## Plugin instance definition

```json
{
  "name": "UrlDiscoveryPlugin",
  "enabled": true,
  "pluginPath": "~appFolder/plugins/dev-proxy-plugins.dll"
}
```

## Configuration example

None

## Configuration properties

None
