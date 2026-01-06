---
title: Use Dev Proxy with SharePoint Framework (SPFx) solutions
description: How to use Dev Proxy with SharePoint Framework (SPFx) solutions
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/03/2026
---

<!-- INTENT: Use Dev Proxy with SharePoint Framework -->
<!-- SOLUTION: Configure gulp-serve with proxy settings -->
<!-- RESULT: SPFx workbench requests routed through Dev Proxy -->
<!-- PLUGINS: various -->
<!-- JOB: intercept-requests -->
<!-- TIME: 10 minutes -->

# Use Dev Proxy with SharePoint Framework (SPFx) solutions

> **At a glance**  
> **Goal:** Use Dev Proxy with SharePoint Framework  
> **Time:** 10 minutes  
> **Plugins:** Various  
> **Prerequisites:** [Set up Dev Proxy](../get-started/set-up.md), SPFx development environment

[SharePoint Framework](/sharepoint/dev/spfx/sharepoint-framework-overview) (SPFx) is a development model for extending SharePoint, Microsoft Teams, Microsoft Viva and Microsoft 365. When you build SPFx solutions, you can use Dev Proxy to intercept web requests made by your solution and see how it handles API errors.

## Intercept web requests made by your SPFx solution

SharePoint Framework solutions are client-side applications that run in the web browser. Before you start Dev Proxy, you need to configure it to intercept requests made by your SPFx solution. Often, you want to intercept requests to Microsoft Graph and SharePoint REST APIs. If you use custom APIs, you want to intercept requests to them as well. You can define the URLs to intercept in the `urlsToWatch` property in the Dev Proxy configuration file.

**File:** devproxyrc.json (urlsToWatch section)

```json
{
  // [...] trimmed for brevity
  "urlsToWatch": [
    "https://graph.microsoft.com/*",
    "https://*.sharepoint.com/*"
    // other URLs to watch
  ]
}
```

> [!TIP]
> When using Dev Proxy with SharePoint Framework Dev Proxy solutions, use the [SPFx preset](https://adoption.microsoft.com/sample-solution-gallery/sample/pnp-devproxy-spfx/) from the Sample Solution Gallery. It contains the common configuration for intercepting web requests made by SPFx solutions, including requests to Microsoft Graph and SharePoint REST APIs.

When you start Dev Proxy on your machine, it automatically intercepts web requests made by your SPFx solution and simulates configured responses. You don't need to change your SPFx solution to use Dev Proxy.

## Configure Dev Proxy to not intercept SharePoint Framework workbench requests

When building SPFx solutions, you use the SharePoint Framework workbench to test your web parts. SharePoint Framework workbench runs in the web browser and uses SharePoint APIs to load web parts. By default, Dev Proxy intercepts all web requests from your web browser, including the requests made by the SharePoint Framework workbench. As a result, it can prevent you from testing your web parts.

To avoid Dev Proxy blocking the requests made by the SharePoint Framework workbench, configure Dev Proxy to not intercept requests to the web part API. In your Dev Proxy configuration file, exclude the API by updating the `urlsToWatch` property.

**File:** devproxyrc.json (urlsToWatch section with exclusion)

```json
{
  // [...] trimmed for brevity
  "urlsToWatch": [
    "!https://*.sharepoint.com/_api/web/GetClientSideComponents*"
    // other URLs to watch
  ]
}
```

> [!TIP]
> If you use the SPFx preset from the Sample Solution Gallery, it already excludes this URL from being intercepted by Dev Proxy.

## See also

- [Use preset configurations](./use-preset-configurations.md)
- [Discover what URLs to intercept](./discover-urls-watch.md)
- [Intercept requests from specific processes](./intercept-requests-from-specific-processes.md)
