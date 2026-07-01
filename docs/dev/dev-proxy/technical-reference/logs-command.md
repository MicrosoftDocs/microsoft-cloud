---
title: logs
description: Dev Proxy logs command reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 07/01/2026
---

<!-- INTENT: Reference for devproxy logs command -->
<!-- COMMAND: devproxy logs -->

# logs

Shows logs from a running Dev Proxy instance.

## Synopsis

```text
devproxy logs [options]

Options:
  -f, --follow             Follow log output
  -n, --lines <N>          Number of lines to show from the end of the log
  --since <time>           Show logs since timestamp (e.g. '2026-01-24T14:00:00'
                           or '5m' for 5 minutes ago)
  --pid <pid>              Show logs from a specific instance
  --log-level <log-level>  Level of messages to log
  --no-color               Disable colored output
  --output <format>        Output format
  -h, --help               Show help
```

## Usage

```console
devproxy logs
```

## Arguments

None

## Options

|Name|Description|Allowed values|Default|
|--|--|--|--|
|`-f`, `--follow`|Follow log output (tail -f style)|n/a|n/a|
|`--lines`, `-n <N>`|Number of lines to show from the end of the log|integer|`50`|
|`--log-level <log-level>`|Level of messages to log|`Trace`, `Debug`, `Information`, `Warning`, `Error`, `Critical`, `None`|`Information`|
|`--no-color`|Disable colored output|n/a|n/a|
|`--output <format>`|Output format|`Text`, `Json`|`Text`|
|`--pid <pid>`|Show logs from a specific Dev Proxy instance by process ID|integer|n/a|
|`--since <time>`|Show logs since timestamp or relative time|timestamp or relative (e.g. `5m`, `2h`)|n/a|

## Remarks

When you run multiple Dev Proxy instances (using `--as-system-proxy false`), use the `--pid` option to show logs from a specific instance.

## See also

- [status](status-command.md)
- [stop](stop-command.md)
- [Use the system proxy option](../how-to/use-system-proxy-option.md)
