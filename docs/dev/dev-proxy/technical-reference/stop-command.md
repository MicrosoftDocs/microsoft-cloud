---
title: stop
description: Dev Proxy stop command reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 07/01/2026
---

<!-- INTENT: Reference for devproxy stop command -->
<!-- COMMAND: devproxy stop -->

# stop

Stops running Dev Proxy instances.

## Synopsis

```text
devproxy stop [options]

Options:
  -f, --force              Force stop the proxy by killing the process
  --pid <pid>              Stop a specific instance
  --log-level <log-level>  Level of messages to log
  --no-color               Disable colored output
  --output <format>        Output format
  -h, --help               Show help
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
|`-f`, `--force`|Forcefully terminate the Dev Proxy process instead of sending a graceful stop signal|n/a|n/a|
|`--log-level <log-level>`|Level of messages to log|`Trace`, `Debug`, `Information`, `Warning`, `Error`, `Critical`, `None`|`Information`|
|`--no-color`|Disable colored output|n/a|n/a|
|`--output <format>`|Output format|`Text`, `Json`|`Text`|
|`--pid <pid>`|Stop a specific Dev Proxy instance by process ID|integer|n/a|

## Remarks

When you run multiple Dev Proxy instances (using `--as-system-proxy false`), the `stop` command without `--pid` stops all running instances. Use the `--pid` option to stop a specific instance.

If the graceful stop fails (for example, because the API port is unavailable), use the `--force` option to forcefully terminate the process.

## See also

- [status](status-command.md)
- [logs](logs-command.md)
- [Use the system proxy option](../how-to/use-system-proxy-option.md)
