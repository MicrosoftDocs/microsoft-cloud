---
title: Exclude a URL
description: How to configure URLs that proxy shouldn't intercept
author: garrytrinder
ms.author: garrytrinder
ms.date: 01/06/2026
---

<!-- INTENT: Prevent Dev Proxy from intercepting specific URLs -->
<!-- SOLUTION: Add URL patterns to urlsToExclude in config -->
<!-- RESULT: Dev Proxy ignores matching URLs -->
<!-- JOB: configure-proxy -->
<!-- TIME: 3 minutes -->

# Exclude a URL

> **At a glance**  
> **Goal:** Prevent Dev Proxy from intercepting specific URLs  
> **Time:** 3 minutes  
> **Prerequisites:** [Set up Dev Proxy](../get-started/set-up.md)

To exclude a URL from being intercepted, prepend it with an `!` (exclamation mark):

**File:** devproxyrc.json

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.0.0/rc.schema.json",
  "plugins": [
    {
      "name": "GenericRandomErrorPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll"
    }
  ],
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

## See also

- [Configure Dev Proxy](../get-started/configure.md) - URL patterns
- [Glossary](../concepts/glossary.md) - Dev Proxy terminology
