---
title: Record and export proxy activity
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

Use record mode to record proxy activity when testing your app.

There are two ways to start recording:

1. **Record immediately**. Start proxy with `--record` option, for example, `m365proxy --record`.
1. **Record adhoc**. Press <kbd>R</kbd> whilst the proxy is running.

When recording is enabled, `? Recording...` is show in the proxy output.

There are two ways to stop recording:

1. **Stop proxy**. Press <kbd>Ctrl</kbd> + <kbd>C</kbd>.
1. **Stop adhoc**. Press <kbd>S</kbd>.

An execution summary report can be also generated when the recording is stopped.

> ℹ️ By default the `ExecutionSummaryPlugin` is disabled. To enable the plugin, open the [m365proxyrc](https://github.com/microsoft/m365-developer-proxy/wiki/m365proxyrc) file, search for the plugin object and change the `enabled` property to `true`. The plugin will be enabled the next time you start proxy.

By default, the summary report is returned to the terminal output.

To save the summary report to a file, start the proxy with the `--summary-file-path` option. The value can be a relative or absolute path.

```sh
m365proxy --summary-file-path report.md
```

By default, proxy reports activities grouped by URL. To group activity by message type, use the `--summary-group-by` option.

```sh
m365proxy --summary-group-by messageType
```

To record all activity immediately, save the report to a file and group by message type, use:

```sh
m365proxy --record --summary-file-path report.md --summary-group-by messageType
```

> Note: All recording is local, no data is sent to Microsoft.
