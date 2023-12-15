---
title: Why are random errors not thrown when using mocks
description: How to fix proxy not throwing random errors when using mocks
author: garrytrinder
ms.author: garrytrinder
ms.date: 11/03/2023
ms.topic: how-to
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
  - tool=devproxy
---

# Why are random errors not thrown when using mocks

You might find that when trying to use random errors and mocks, proxy isn't returning random errors.

By default, the [devproxyrc](../technical-reference/devproxyrc.md) configuration is similar to:

```json
{
  "plugins": [
    // [...] trimmed for brevity
    {
      "name": "GraphMockResponsePlugin",
      "enabled": true,
      "pluginPath": "plugins\\dev-proxy-plugins.dll",
      "configSection": "mocksPlugin"
    },
    {
      "name": "GraphRandomErrorPlugin",
      "enabled": true,
      "pluginPath": "plugins\\dev-proxy-plugins.dll",
      "configSection": "graphRandomErrorsPlugin"
    }
    // [...] trimmed for brevity
  ],
  // [...] trimmed for brevity
}
```

Proxy executes plugins in the order they're defined in the configuration. In this case, mocks are executed before random errors so if you have a mock defined for a URL, the request never reaches the random error plugin.

If you want both random errors and mocks, change the order of plugins to:

```json
{
  "plugins": [
    // [...] trimmed for brevity
    {
      "name": "GraphRandomErrorPlugin",
      "enabled": true,
      "pluginPath": "plugins\\dev-proxy-plugins.dll",
      "configSection": "graphRandomErrorsPlugin"
    },
    {
      "name": "GraphMockResponsePlugin",
      "enabled": true,
      "pluginPath": "plugins\\dev-proxy-plugins.dll",
      "configSection": "mocksPlugin"
    }
    // [...] trimmed for brevity
  ],
  // [...] trimmed for brevity
}
```

This way random errors are handled first, and any request that proxy doesn't randomly fail, is compared against mocks.
