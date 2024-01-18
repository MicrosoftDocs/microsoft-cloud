---
title: Why are random errors not thrown when using mocks
description: How to fix proxy not throwing random errors when using mocks
author: garrytrinder
ms.author: garrytrinder
ms.date: 01/18/2024
---

# Why are random errors not thrown when using mocks

You might find that when trying to use random errors and mocks, proxy isn't returning random errors. One of the reasons could be the incorrect order of plugins in the [devproxyrc](../technical-reference/devproxyrc.md) configuration.

Proxy executes plugins in the order they're defined in the configuration. In this case, mocks are executed before random errors so if you have a mock defined for a URL, the request never reaches the random error plugin.

If you want both random errors and mocks, change the order of plugins to:

```json
{
  "plugins": [
    // [...] trimmed for brevity
    {
      "name": "GenericRandomErrorPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/dev-proxy-plugins.dll",
      "configSection": "genericRandomErrorPlugin"
    },
    {
      "name": "MockResponsePlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/dev-proxy-plugins.dll",
      "configSection": "mocksPlugin"
    }
    // [...] trimmed for brevity
  ],
  // [...] trimmed for brevity
}
```

This way random errors are handled first, and any request that proxy doesn't randomly fail, is compared against mocks.
