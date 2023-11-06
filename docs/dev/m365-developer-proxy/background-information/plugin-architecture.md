In v0.4, we refactored the Microsoft 365 Developer Proxy to use a Plugin based architecture.

A plugin is a piece of code (DLL) that determines proxy behaviour. The proxy executes the plugin code at runtime. Developers can write custom plugins to provide behaviours of their own custom APIs.

The [m365proxyrc.json](./m365proxyrc) file contains the plugin configuration.

The proxy has standard plugins that can be used with any API, as well as plugins specifically for use with the Microsoft Graph API.

The standard plugins are:

- **MockResponsePlugin**, provides the ability to respond to requests with a mocked response.
- **RateLimitingPlugin**, provides the ability to simulate behaviours of APIs that return `Rate-Limit` headers in responses.
- **ODataPluginGuidance**, provides guidance to help developers correctly handle retrieving multiple pages of data.
- **ExecutionSummaryPlugin**, provides the ability to export proxy activity to a summary report.
- **GenericRandomErrorPlugin**, provides the ability to fail requests.

Example configuration of the `MockResponsePlugin`:

```json
{
    "name": "MockResponsePlugin",
    "enabled": true,
    "pluginPath": "plugins\\m365-developer-proxy-plugins.dll",
    "configSection": "mocksPlugin"
}
```

The `configSection` takes a reference to a configuration object. The `mocksPlugin` object describes the name of the file containing mocked responses.

```json
"mocksPlugin": {
    "mocksFile": "responses.json"
}
```

There are five Microsoft Graph API plugins provided are:

- **GraphRandomErrorPlugin**, provides random error responses for Microsoft Graph. It returns a random error response from a list of [supported HTTP codes](./Supported-HTTP-error-status-codes).
- **GraphSdkGuidancePlugin**, provides guidance when a Microsoft Graph SDK is not used.
- **GraphSelectGuidancePlugin**, provides guidance when the `$select` query-string parameter is not used on a GET request.
- **GraphBetaSupportGuidancePlugin**, provides guidance when a Microsoft Graph v1.0 endpoint is not used.
- **GraphClientRequestIdGuidancePlugin**, provides guidance when the `client-request-id` request header is not used.

Example configuration of the `GraphSelectGuidancePlugin`:

```json
{
    "name": "GraphBetaSupportGuidancePlugin",
    "enabled": true,
    "pluginPath": "plugins\\m365-developer-proxy-plugins.dll",
    "urlsToWatch": [
        "https://graph.microsoft.com/beta/*",
        "https://graph.microsoft.us/beta/*",
        "https://dod-graph.microsoft.us/beta/*",
        "https://microsoftgraph.chinacloudapi.cn/beta/*"
    ]
}
```

Plugins watch requests made to URLs in the global `urlsToWatch` array. But, each plugin can override this list for their own needs. In the above example, the `GraphBetaSupportGuidancePlugin` will only watch on Microsoft Graph Beta endpoint URLs.
