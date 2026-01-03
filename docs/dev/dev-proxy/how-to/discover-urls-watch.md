---
title: Discover URLs to watch
description: How to find out which URLs your app is calling and how to configure the Dev Proxy to simulate behaviors for them.
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/03/2026
---

<!-- INTENT: Find which URLs my app calls to configure Dev Proxy -->
<!-- SOLUTION: Enable UrlDiscoveryPlugin and run app normally -->
<!-- RESULT: Report listing all URLs the app called -->
<!-- PLUGINS: UrlDiscoveryPlugin, PlainTextReporter -->
<!-- JOB: analyze-usage -->
<!-- TIME: 5 minutes -->

# Discover URLs to watch

> **At a glance**  
> **Goal:** Identify which URLs your app calls to configure Dev Proxy  
> **Time:** 5 minutes  
> **Plugins:** [UrlDiscoveryPlugin](../technical-reference/urldiscoveryplugin.md), [PlainTextReporter](../technical-reference/plaintextreporter.md)  
> **Prerequisites:** [Set up Dev Proxy](../get-started/set-up.md)

Dev Proxy allows you to simulate behaviors for APIs. To do that, you need to know which URLs your app is calling and configure Dev Proxy to intercept them.

To find out which URLs your app is calling, use the `--discover` option. This option loads a preset that uses the [`UrlDiscoveryPlugin`](../technical-reference/urldiscoveryplugin.md) and [`PlainTextReporter`](../technical-reference/plaintextreporter.md) to create a list of URLs that the proxy intercepts.

The discovery preset is configured to intercept requests to any URL and pass them through to the original API. It uses the `UrlDiscoveryPlugin`, to generate a list of unique URLs, and the `PlainTextReporter` to save the list to a text file.

> [!TIP]
> Before you start Dev Proxy with the `--discover` option, find out from which process you want to capture requests. You can specify the process by its ID or name. Without this option, Dev Proxy intercepts all requests made by your machine, which makes it hard to find the URLs you're interested in. For more information, see [Intercept requests from specific processes](./intercept-requests-from-specific-processes.md).

For example, to discover URLs that a client-side application uses, running in Microsoft Edge on Windows, run the following command:

```console
devproxy --discover --watch-process-names msedge
```

After you start Dev Proxy, interact with your application so that it issues requests to the APIs you want to simulate. Dev Proxy intercepts these requests. When you finish, stop Dev Proxy by pressing `Ctrl+C`. The `urls-to-watch` preset saves the list of URLs to the `UrlDiscoveryPlugin_PlainTextReporter.txt` file in the current directory.

## Next steps

Learn more about the UrlDiscoveryPlugin.

> [!div class="nextstepaction"]
> [UrlDiscoveryPlugin](../technical-reference/urldiscoveryplugin.md)

## See also

- [Intercept requests from specific processes](./intercept-requests-from-specific-processes.md) - Filter by process
- [UrlDiscoveryPlugin](../technical-reference/urldiscoveryplugin.md) - Full reference
- [Glossary](../concepts/glossary.md) - Dev Proxy terminology
