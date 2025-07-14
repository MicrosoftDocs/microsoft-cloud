---
title: Resolve relative paths
description: How Dev Proxy resolves relative paths
author: garrytrinder
ms.author: garrytrinder
ms.date: 04/08/2024
---

# Resolve relative paths

All file paths used in configuration files (`devproxyrc.json`) are relative to the location of the configuration file.

In the below example, the `pluginPath` is relative to the configuration file. The default `devproxyrc.json` configuration file is located in the same location where the `plugins` folder exists.

```json
{
  "name": "GraphSelectGuidancePlugin",
  "enabled": true,
  "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
  "urlsToWatch": [
    "https://graph.microsoft.com/v1.0/*",
    ...
  ]
}
```

Let's say you start the proxy from a different directory, such as from the location of a project you're working in.

If an `devproxyrc.json` configuration file exists in the current directory, then the file path resolution is relative to this file. If not, the proxy falls back to the default `devproxyrc.json` file.

Using a configuration file that isn't located in the proxy installation directory requires you to ensure that plugin paths are resolved correctly.

Use the `~appFolder` token in the file paths to ensure that the path is prepended with the absolute path to the proxy installation directory.

```json
{
  "name": "GraphSelectGuidancePlugin",
  "enabled": true,
  "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
  "urlsToWatch": [
    "https://graph.microsoft.com/v1.0/*",
    ...
  ]
}
```

The `~appFolder` token can be used in any path used by the proxy.

Let's say you want to load a preset configuration, you can use the `~appFolder` token in the path to reference the configuration file located in the proxy installation directory.

```console
devproxy --config-file ~appFolder/config/microsoft-graph.json
```
