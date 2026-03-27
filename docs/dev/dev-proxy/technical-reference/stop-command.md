---
title: stop
description: Dev Proxy stop command reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 03/26/2026
---

<!-- INTENT: Reference for devproxy stop command -->
<!-- COMMAND: devproxy stop -->

# stop

Stops running Dev Proxy instances.

## Synopsis

```text
devproxy stop [options]

Options:
  --pid <pid>            Stop a specific instance
  --force                Forcefully terminate the process
  --output <format>      Output format: text|json (default: text)
  --log-level <level>    Logging level: trace|debug|information|warning|error
  -h, --help             Show help
```

## Usage

```console
devproxy stop
```

## Arguments

None

## Options

|Name|Description|Allowed values|Default|
|--|--|--|--|
|`--force`|Forcefully terminate the Dev Proxy process instead of sending a graceful stop signal|n/a|n/a|
|`--log-level <loglevel>`|Level of messages to log|`trace`, `debug`, `information`, `warning`, `error`|`information`|
|`--output <format>`|Output format for structured logging|`text`, `json`|`text`|
|`--pid <pid>`|Stop a specific Dev Proxy instance by process ID|integer|n/a|

## Remarks

When you run multiple Dev Proxy instances (using `--as-system-proxy false`), the `stop` command without `--pid` stops all running instances. Use the `--pid` option to stop a specific instance.

If the graceful stop fails (for example, because the API port is unavailable), use the `--force` option to forcefully terminate the process.

## See also

- [status](status-command.md)
- [logs](logs-command.md)
- [Use the system proxy option](../how-to/use-system-proxy-option.md)
