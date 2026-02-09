---
title: Change request failure rate
description: How to configure the request failure rate
author: garrytrinder
ms.author: garrytrinder
ms.date: 02/07/2026
---

<!-- INTENT: Change how often requests fail with simulated errors -->
<!-- SOLUTION: Use --failure-rate option or rate config setting -->
<!-- RESULT: Dev Proxy fails requests at specified percentage -->
<!-- JOB: test-error-handling -->
<!-- TIME: 2 minutes -->

# Change request failure rate

> **At a glance**  
> **Goal:** Configure how often Dev Proxy simulates API failures  
> **Time:** 2 minutes  
> **Prerequisites:** [Set up Dev Proxy](../get-started/set-up.md)

By default, there's 50% chance that Dev Proxy returns a random error for your API. You can change the likelihood to a different value using the `--failure-rate` option, for example:

```console
devproxy --failure-rate 80
```

Alternatively, you can configure the failure rate in the Dev Proxy configuration file. Dev Proxy automatically reloads its configuration when you save changes, so you don't need to restart the proxy.

**File:** devproxyrc.json

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/rc.schema.json",
  "plugins": [
    {
      "name": "GenericRandomErrorPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "genericRandomErrorPlugin"
    }
  ],
  "urlsToWatch": [
    "https://api.example.com/*"
  ],
  "genericRandomErrorPlugin": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/genericrandomerrorplugin.schema.json",
    "errorsFile": "errors.json",
    "rate": 80
  }
}
```

> [!IMPORTANT]
> When you configure the failure rate to 0, Dev Proxy will pass all requests through to the original API. When you configure it to 100, Dev Proxy will simulate an error for every matching request.

## See also

- [Test my app with random errors](./test-my-app-with-random-errors.md) - Error simulation workflow
- [Glossary](../concepts/glossary.md) - Dev Proxy terminology
