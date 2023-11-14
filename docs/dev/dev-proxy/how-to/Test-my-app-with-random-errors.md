---
title: Test my app with random errors
description: How to test your app with random errors
author: garrytrinder
ms.author: garrytrinder
ms.contributors: garrytrinder
ms.date: 11/03/2023
ms.topic: conceptual
ms.service: microsoft-cloud-for-developers

categories:
  - developer-tools
products:
  - microsoft-365
  - microsoft-graph
  - sharepoint-online
  - m365
ms.custom:
  - fcp
  - team=cloud_advocates
---

# Test my app with random errors

When applications use Microsoft Graph and other cloud services, it can happen, that these APIs are temporarily unavailable. It's important, that applications consider such scenario and handle exceptions properly.

Testing exceptions in APIs you don't manage is hard, because it's hard to make the API return a specific response. Using Dev Proxy, you can mimic erroneous responses from Microsoft Graph and test your application to see that it handles these errors properly.

> [!TIP]
> The [GraphRandomErrorPlugin](../technical-reference/GraphRandomErrorPlugin.md) controls the behavior of returning random errors. In older releases of the proxy you may find that the `GraphRandomErrorPlugin` may not be in your [devproxyrc.json](../technical-reference/devproxyrc.md) file. Add the [plugin and configuration objects](../technical-reference/GraphRandomErrorPlugin.md) to your `devproxyrc.json` file. The plugin will be enabled the next time you start proxy.

To test your application, start the Dev Proxy:

```sh
devproxy --no-mocks
```

By default, Dev Proxy randomly responds with 429, 500, 502, 503, 504, or 507 errors. In its default configuration, there's a 50% chance that the proxy will intercept a request to Microsoft Graph. You can increase the likelihood to a higher value using the `--failure-rate` option, for example:

```sh
devproxy --no-mocks --failure-rate 80
```

After starting the proxy, run your application and see how it responds to the different errors.

## Microsoft Graph Batch Support

In v0.12, we introduced support for failing requests sent in batch requests to Microsoft Graph with a random error.

There are no special requirements for failing requests sent in a batch, a `424 Failed Dependency` response is returned.
