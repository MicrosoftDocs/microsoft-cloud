---
title: status
description: Dev Proxy status command reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 03/26/2026
---

<!-- INTENT: Reference for devproxy status command -->
<!-- COMMAND: devproxy status -->

# status

Shows the status of running Dev Proxy instances.

## Synopsis

```text
devproxy status [options]

Options:
  --pid <pid>            Show status of a specific instance
  --output <format>      Output format: text|json (default: text)
  --log-level <level>    Logging level: trace|debug|information|warning|error
  -h, --help             Show help
```

## Usage

```console
devproxy status
```

## Arguments

None

## Options

|Name|Description|Allowed values|Default|
|--|--|--|--|
|`--log-level <loglevel>`|Level of messages to log|`trace`, `debug`, `information`, `warning`, `error`|`information`|
|`--output <format>`|Output format for structured logging|`text`, `json`|`text`|
|`--pid <pid>`|Show status of a specific Dev Proxy instance by process ID|integer|n/a|

## Remarks

When you run multiple Dev Proxy instances (using `--as-system-proxy false`), the `status` command lists all running instances. Use the `--pid` option to show the status of a specific instance.

## See also

- [stop](stop-command.md)
- [logs](logs-command.md)
- [Use the system proxy option](../how-to/use-system-proxy-option.md)
