---
title: devproxyrc
description: devproxyrc.json file reference
author: garrytrinder
ms.author: garrytrinder
ms.date: 01/25/2024
---

# devproxyrc

Default configuration.

```json
{
  "$schema": "https://raw.githubusercontent.com/microsoft/dev-proxy/main/schemas/v0.14.1/rc.schema.json",
  "plugins": [
    {
      "name": "RetryAfterPlugin",
      "enabled": true,
      "pluginPath": "plugins\\dev-proxy-plugins.dll"
    },
    {
      "name": "GenericRandomErrorPlugin",
      "enabled": true,
      "pluginPath": "plugins\\dev-proxy-plugins.dll",
      "configSection": "genericRandomErrorPlugin"
    }
  ],
  "urlsToWatch": [
    "https://jsonplaceholder.typicode.com/*"
  ],
  "genericRandomErrorPlugin": {
    "errorsFile": "devproxy-errors.json"
  },
  "rate": 50,
  "labelMode": "text",
  "logLevel": "info"
}
```
