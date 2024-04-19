---
title: devproxyrc
description: devproxyrc.json file reference
author: garrytrinder
ms.author: garrytrinder
ms.date: 04/19/2024
---

# devproxyrc

Default configuration.

```json
{
  "$schema": "https://raw.githubusercontent.com/microsoft/dev-proxy/main/schemas/v0.17.0/rc.schema.json",
  "plugins": [
    {
      "name": "RetryAfterPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/dev-proxy-plugins.dll"
    },
    {
      "name": "GenericRandomErrorPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/dev-proxy-plugins.dll",
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
  "logLevel": "information",
  "newVersionNotification": "stable"
}
```
