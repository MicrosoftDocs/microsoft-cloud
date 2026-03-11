---
title: devproxyrc
description: devproxyrc.json file reference
author: garrytrinder
ms.author: garrytrinder
ms.date: 03/05/2026
---

<!-- INTENT: Reference for devproxyrc.json configuration file structure -->

# devproxyrc

Default Dev Proxy configuration file.

Dev Proxy supports both JSON (`.json`) and YAML (`.yaml`, `.yml`) formats for configuration files. The default file is `devproxyrc.json`, but Dev Proxy also autodiscovers `devproxyrc.yaml` and `devproxyrc.yml`.

> [!NOTE]
> Schema validation (`validateSchemas`) only applies to JSON-based configuration files. YAML configuration files aren't validated against schemas at runtime.

**File:** devproxyrc.json

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.2.0/rc.schema.json",
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
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.2.0/genericrandomerrorplugin.schema.json",
    "errorsFile": "devproxy-errors.json",
    "rate": 50
  },
  "logLevel": "information",
  "newVersionNotification": "stable",
  "showSkipMessages": true,
  "showTimestamps": true,
  "validateSchemas": true
}
```

**File:** devproxyrc.yaml (equivalent YAML configuration)

```yaml
plugins:
  - name: RetryAfterPlugin
    enabled: true
    pluginPath: "~appFolder/plugins/DevProxy.Plugins.dll"
  - name: GenericRandomErrorPlugin
    enabled: true
    pluginPath: "~appFolder/plugins/DevProxy.Plugins.dll"
    configSection: genericRandomErrorPlugin

urlsToWatch:
  - "https://jsonplaceholder.typicode.com/*"

genericRandomErrorPlugin:
  errorsFile: devproxy-errors.json
  rate: 50

logLevel: information
newVersionNotification: stable
showSkipMessages: true
showTimestamps: true
validateSchemas: true
```

YAML configuration supports anchors and merge keys for reusable configuration blocks:

```yaml
# Define reusable response templates using YAML anchors
throttled: &throttled
  statusCode: 429
  body: '{"error": "Too many requests"}'

mocks:
  - request:
      url: https://api.example.com/users
    response:
      <<: *throttled
  - request:
      url: https://api.example.com/groups
    response:
      <<: *throttled
```
