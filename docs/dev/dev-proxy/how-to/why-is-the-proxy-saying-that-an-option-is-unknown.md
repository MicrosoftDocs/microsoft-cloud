---
title: Why is the proxy saying that an option is unknown
description: Why is the proxy saying that an option is unknown
author: garrytrinder
ms.author: garrytrinder
ms.date: 01/03/2026
---

<!-- INTENT: Fix unknown option error -->
<!-- SOLUTION: Update Dev Proxy to latest version -->
<!-- RESULT: Unknown option error resolved -->
<!-- PLUGINS: various -->
<!-- JOB: troubleshoot -->
<!-- TIME: 3 minutes -->

# Why is the proxy saying that an option is unknown

> **At a glance**  
> **Goal:** Fix unknown option error  
> **Time:** 3 minutes  
> **Plugins:** Various  
> **Prerequisites:** [Set up Dev Proxy](../get-started/set-up.md)

Dev Proxy uses a plugin architecture, as such, proxy features can be added, removed, enabled or disabled through changing the configuration in the [devproxyrc.json](../technical-reference/devproxyrc.md) file.

Some plugins provide command line options. When the plugin is disabled, the proxy isn't able to interpret the command line options that the plugin provides.

By default, we ship with many plugins that are configured but disabled.

For example, the [ExecutionSummaryPlugin](../technical-reference/ExecutionSummaryPlugin.md) records proxy activity and exposes command line options, such as `--summary-file-path`. As the plugin is disabled, the `--summary-file-path` option is an unknown option and so the proxy returns an error.

To enable a plugin and make its options available for use, open the [devproxyrc.json](../technical-reference/devproxyrc.md) file, locate the plugin and set `enabled` to `true`.

**File:** devproxyrc.json (plugin entry in plugins array)

```json
{
  "name": "ExecutionSummaryPlugin",
  "enabled": true,
  "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
  "configSection": "executionSummaryPlugin"
}
```

## See also

- [Dev Proxy configuration file](../technical-reference/devproxyrc.md)
- [Plugin architecture](../technical-reference/plugin-architecture.md)
- [Get help and support](./get-help-and-support.md)
