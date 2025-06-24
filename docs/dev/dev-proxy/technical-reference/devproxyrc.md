---
title: devproxyrc
description: devproxyrc.json file reference
author: garrytrinder
ms.author: garrytrinder
ms.date: 04/30/2025
---

# devproxyrc

Default Dev Proxy configuration file.

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v0.29.0/rc.schema.json",
  "plugins": [
    {
      "name": "RetryAfterPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll"
    },
    {
      "name": "GenericRandomErrorPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "genericRandomErrorPlugin"
    }
  ],
  "urlsToWatch": [
    "https://jsonplaceholder.typicode.com/*"
  ],
  "genericRandomErrorPlugin": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v0.27.0/genericrandomerrorplugin.schema.json",
    "errorsFile": "devproxy-errors.json",
    "rate": 50,
  },
  "logLevel": "information",
  "newVersionNotification": "stable",
  "showSkipMessages": true,
  "showTimestamps": true,
  "validateSchemas": true
}
```
