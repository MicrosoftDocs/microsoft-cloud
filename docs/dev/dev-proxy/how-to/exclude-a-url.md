---
title: Exclude a URL
description: How to configure URLs that proxy shouldn't intercept
author: garrytrinder
ms.author: garrytrinder
ms.date: 04/30/2025
---

# Exclude a URL

To exclude a URL from being intercepted, prepend it with an `!` (exclamation mark):

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v0.29.1/rc.schema.json",
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
  ]
}
```

In the above example, the proxy ignores any requests made to `/_api/web/GetClientSideComponents`.

When excluding URLs, keep in mind that proxy looks for matching URLs in the order in which they’re defined in the configuration.

If you want to exclude specific URLs, you should define them first, before more global URL matches.
