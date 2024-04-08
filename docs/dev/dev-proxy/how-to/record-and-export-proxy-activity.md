---
title: Record and export proxy activity
description: How to record and export proxy activity
author: garrytrinder
ms.author: garrytrinder
ms.date: 04/08/2024
---

# Record and export proxy activity

Use record mode to record proxy activity when testing your app.

There are two ways to start recording:

1. **Record immediately**. Start proxy with `--record` option, for example, `devproxy --record`.
1. **Record adhoc**. Press <kbd>R</kbd> whilst the proxy is running.

When recording is enabled, `? Recording...` is show in the proxy output.

There are two ways to stop recording:

1. **Stop proxy**. Press <kbd>Ctrl</kbd> + <kbd>C</kbd>.
1. **Stop adhoc**. Press <kbd>S</kbd>.

An execution summary report can be also generated when the recording is stopped.

> [!NOTE]
> By default the [`ExecutionSummaryPlugin`](../technical-reference/executionsummaryplugin.md) is disabled. To get a summary report, enable the plugin in the Dev Proxy configuration file and restart  proxy.

By default, the summary report is returned to the command prompt output.

To save the summary report to a file, start the proxy with the `--summary-file-path` option. The value can be a relative or absolute path.

```console
devproxy --summary-file-path report.md
```

By default, proxy reports activities grouped by URL. To group activity by message type, use the `--summary-group-by` option.

```console
devproxy --summary-group-by messageType
```

To record all activity immediately, save the report to a file and group by message type, use:

```console
devproxy --record --summary-file-path report.md --summary-group-by messageType
```

> Note: All recording is local, no data is sent to Microsoft.
