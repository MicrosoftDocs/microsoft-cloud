---
title: Record and export proxy activity
description: How to record and export proxy activity
author: garrytrinder
ms.author: garrytrinder
ms.date: 01/03/2026
---

<!-- INTENT: Record and export Dev Proxy activity to a report file -->
<!-- SOLUTION: Enable ExecutionSummaryPlugin with MarkdownReporter -->
<!-- RESULT: Markdown file with summary of all intercepted requests -->
<!-- PLUGINS: ExecutionSummaryPlugin, MarkdownReporter -->
<!-- JOB: analyze-usage -->
<!-- TIME: 10 minutes -->

# Record and export proxy activity

> **At a glance**  
> **Goal:** Record and export Dev Proxy activity to a report file  
> **Time:** 10 minutes  
> **Plugins:** [ExecutionSummaryPlugin](../technical-reference/executionsummaryplugin.md), [MarkdownReporter](../technical-reference/markdownreporter.md)  
> **Prerequisites:** [Set up Dev Proxy](../get-started/set-up.md)

To record and export proxy activity, use the [ExecutionSummaryPlugin](../technical-reference/executionsummaryplugin.md) and a reporter plugin in your configuration file.

The following example shows how to configure the Dev Proxy to record and export proxy activity using the [ExecutionSummaryPlugin](../technical-reference/executionsummaryplugin.md) and the [MarkdownReporter](../technical-reference/markdownreporter.md) plugin.

**File:** devproxyrc.json

```json
{
  "plugins": [
    {
      "name": "ExecutionSummaryPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll"
    },
    {
        "name": "MarkdownReporter",
        "enabled": true,
        "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll"
    }
  ],
  "urlsToWatch": [
    "https://jsonplaceholder.typicode.com/*"
  ]
}
```

> [!NOTE]
> To export the activity, the reporter plugin must be enabled in the configuration file and placed after the `ExecutionSummaryPlugin` in the plugins list. It is recommended to place a reporter plugin at the end of the plugins list.

To record activity, Dev Proxy must be placed in record mode.

There are two ways to start recording:

- **Record immediately**. Start proxy with `--record` option, for example, `devproxy --record`.
- **Record adhoc**. Press <kbd>R</kbd> while proxy is running.

When recording is enabled, `? Recording...` is shown in the proxy output.

To generate a report from the recorded activity, stop recording.

There are two ways to stop recording:

- **Stop proxy**. Press <kbd>Ctrl</kbd> + <kbd>C</kbd>.
- **Stop adhoc**. Press <kbd>S</kbd>.

By default, activities grouped by URL. To group activity by message type, use the `--summary-group-by` option.

```console
devproxy --record --summary-group-by messageType
```

> [!NOTE]
> All recording is local. No data is sent to Microsoft.

## See also

- [ExecutionSummaryPlugin](../technical-reference/executionsummaryplugin.md) - Full reference
- [MarkdownReporter](../technical-reference/markdownreporter.md) - Report format
- [Glossary](../concepts/glossary.md) - Dev Proxy terminology
