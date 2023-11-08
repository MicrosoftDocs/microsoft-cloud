---
title: Resolve relative paths
description: Get started with Microsoft 365 Developer Proxy
author: garrytrinder
ms.author: garrytrinder
ms.contributors: garrytrinder
ms.date: 11/03/2023
ms.topic: conceptual
ms.service: microsoft-cloud-for-developers

categories:
  - developer-tools
products:
  - microsoft-365
  - microsoft-graph
  - sharepoint-online
  - m365
ms.custom:
  - fcp
  - team=cloud_advocates
---

All file paths used in configuration files (`m365proxyrc.json`) are relative to the location of the configuration file.

In the below example, the `pluginPath` is relative to the configuration file. The default `m365proxyrc.json` configuration file is located in the same location where the `plugins` folder exists.

```json
{
    "name": "GraphSelectGuidancePlugin",
    "enabled": true,
    "pluginPath": "plugins\\m365-developer-proxy-plugins.dll",
    "urlsToWatch": [
        "https://graph.microsoft.com/v1.0/*",
        ...
    ]
},
```

Let's say you start the proxy from a different directory, such as from the location of a project you're working in.

If an `m365proxyrc.json` configuration file exists in the current directory, then the file path resolution is relative to this file. If not, the proxy falls back to the default `m365proxyrc.json` file.

Using a configuration file that isn't located in the proxy installation directory requires you to ensure that plugin paths are resolved correctly.

Use the `~appFolder` token in the file paths to ensure that the path is prepended with the absolute path to the proxy installation directory.

```json
{
    "name": "GraphSelectGuidancePlugin",
    "enabled": true,
    "pluginPath": "~appFolder\\plugins\\m365-developer-proxy-plugins.dll",
    "urlsToWatch": [
        "https://graph.microsoft.com/v1.0/*",
        ...
    ]
},
```

The `~appFolder` token can be used in any path used by the proxy.

Let's say you want to load a preset configuration, you can use the `~appFolder` token in the path to reference the configuration file located in the proxy installation directory.

```sh
m365proxy --config-file ~appFolder/presets/microsoft-graph.json
```
