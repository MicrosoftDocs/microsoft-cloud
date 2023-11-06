To exclude a URL from being intercepted, prepend it with an `!` (exclamation mark): 

```json
"urlsToWatch": [
    "https://graph.microsoft.com/v1.0/*",
    "https://graph.microsoft.com/beta/*",
    "https://graph.microsoft.us/v1.0/*",
    "https://graph.microsoft.us/beta/*",
    "https://dod-graph.microsoft.us/v1.0/*",
    "https://dod-graph.microsoft.us/beta/*",
    "https://microsoftgraph.chinacloudapi.cn/v1.0/*",
    "https://microsoftgraph.chinacloudapi.cn/beta/*",
    "!https://*.sharepoint.*/*_api/web/GetClientSideComponents",
    "https://*.sharepoint.*/*_api/*",
    "https://*.sharepoint.*/*_vti_bin/*",
    "https://*.sharepoint-df.*/*_api/*",
    "https://*.sharepoint-df.*/*_vti_bin/*"
],
```

In the above example, any requests made to `/_api/web/GetClientSideComponents` will be ignored.

When excluding URLs, keep in mind that proxy will look for matching URLs in the order in which they’re defined in the configuration. 

If you want to exclude specific URLs, you should define them first, before more global URL matches.