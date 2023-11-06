You may find that when trying to use random errors and mocks, random errors are not thrown by proxy.

By default, the [m365proxyrc](./m365proxyrc) configuration is similar to:

```json
{
  "plugins": [
    // [...] trimmed for brevity
    {
      "name": "GraphMockResponsePlugin",
      "enabled": true,
      "pluginPath": "plugins\\m365-developer-proxy-plugins.dll",
      "configSection": "mocksPlugin"
    },
    {
      "name": "GraphRandomErrorPlugin",
      "enabled": true,
      "pluginPath": "plugins\\m365-developer-proxy-plugins.dll",
      "configSection": "graphRandomErrorsPlugin"
    }
    // [...] trimmed for brevity
  ],
  // [...] trimmed for brevity
}
```

Proxy executes plugins in the order they're defined in the configuration. In this case, mocks are executed before random error so if you have a mock defined for a URL, it will never reach the random error.

If you want both random errors and mocks, change the order of plugins to:

```json
{
  "plugins": [
    // [...] trimmed for brevity
    {
      "name": "GraphRandomErrorPlugin",
      "enabled": true,
      "pluginPath": "plugins\\m365-developer-proxy-plugins.dll",
      "configSection": "graphRandomErrorsPlugin"
    },
    {
      "name": "GraphMockResponsePlugin",
      "enabled": true,
      "pluginPath": "plugins\\m365-developer-proxy-plugins.dll",
      "configSection": "mocksPlugin"
    }
    // [...] trimmed for brevity
  ],
  // [...] trimmed for brevity
}
```

This way random errors will be handled first, and any request that's not randomly failed, will be compared against mocks.