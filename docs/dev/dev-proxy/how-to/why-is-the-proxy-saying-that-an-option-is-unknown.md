---
title: Why is the proxy saying that an option is unknown
description: Why is the proxy saying that an option is unknown
author: garrytrinder
ms.author: garrytrinder
ms.date: 01/18/2024
---

# Why is the proxy saying that an option is unknown

Dev Proxy uses a plugin architecture, as such, proxy features can be added, removed, enabled or disabled through changing the configuration in the [devproxyrc.json](../technical-reference/devproxyrc.md) file.

Some plugins provide command line options. When the plugin is disabled, the proxy isn't able to interpret the command line options that the plugin provides.

By default, we ship with many plugins that are configured but disabled.

For example, the [ExecutionSummaryPlugin](../technical-reference/ExecutionSummaryPlugin.md) records proxy activity and exposes command line options, such as `--summary-file-path`. As the plugin is disabled, the `--summary-file-path` option is an unknown option and so the proxy returns an error.

To enable a plugin and make its options available for use, open the [devproxyrc.json](../technical-reference/devproxyrc.md) file, locate the plugin and set `enabled` to `true`.

```json
{
  "name": "ExecutionSummaryPlugin",
  "enabled": true,
  "pluginPath": "~appFolder/plugins/dev-proxy-plugins.dll",
  "configSection": "executionSummaryPlugin"
}
```
