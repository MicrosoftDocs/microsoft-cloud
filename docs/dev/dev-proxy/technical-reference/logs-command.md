---
title: logs
description: Dev Proxy logs command reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 03/26/2026
---

<!-- INTENT: Reference for devproxy logs command -->
<!-- COMMAND: devproxy logs -->

# logs

Shows logs from a running Dev Proxy instance.

## Synopsis

```text
devproxy logs [options]

Options:
  --pid <pid>            Show logs from a specific instance
  --output <format>      Output format: text|json (default: text)
  --log-level <level>    Logging level: trace|debug|information|warning|error
  -h, --help             Show help
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
|`--log-level <loglevel>`|Level of messages to log|`trace`, `debug`, `information`, `warning`, `error`|`information`|
|`--output <format>`|Output format for structured logging|`text`, `json`|`text`|
|`--pid <pid>`|Show logs from a specific Dev Proxy instance by process ID|integer|n/a|

## Remarks

When you run multiple Dev Proxy instances (using `--as-system-proxy false`), use the `--pid` option to show logs from a specific instance.

## See also

- [status](status-command.md)
- [stop](stop-command.md)
- [Use the system proxy option](../how-to/use-system-proxy-option.md)
