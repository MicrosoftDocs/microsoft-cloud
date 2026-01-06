---
title: UrlDiscoveryPlugin
description: UrlDiscoveryPlugin reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/06/2026
---

<!-- INTENT: Discover which URLs an app calls -->
<!-- PLUGIN-TYPE: Reporting -->
<!-- WORKS-WITH: MarkdownReporter, JsonReporter, PlainTextReporter -->
<!-- USE-WHEN: Learning which APIs an app uses before configuring Dev Proxy -->

# UrlDiscoveryPlugin

Creates a list of URLs that Dev Proxy intercepted. Use this plugin to learn which URLs your app is calling and how to configure the Dev Proxy to simulate behaviors for them.

> [!TIP]
> To save the summary to a file, use a reporter plugin in your configuration.

:::image type="content" source="../media/url-discovery-plugin.png" alt-text="Screenshot of a split command prompt window. On the left side, Dev Proxy output showing intercepted requests. On the right side, generated report with intercepted URLs." lightbox="../media/url-discovery-plugin.png":::

## Configuration example

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.0.0/rc.schema.json",
  "plugins": [
    {
      "name": "UrlDiscoveryPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll"
    }
  ]
}
```

## Configuration properties

None

## Next step

> [!div class="nextstepaction"]
> [Discover URLs to watch](../how-to/discover-urls-watch.md)
