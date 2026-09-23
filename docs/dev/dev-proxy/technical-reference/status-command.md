---
title: status
description: Dev Proxy status command reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 09/23/2026
---

<!-- INTENT: Reference for devproxy status command -->
<!-- COMMAND: devproxy status -->

# status

Shows the status of running Dev Proxy instances.

## Synopsis

```text
devproxy status [options]

Options:
  --pid <pid>              Show status of a specific instance
  --log-level <log-level>  Level of messages to log
  --no-color               Disable colored output
  --output <format>        Output format
  -h, --help               Show help
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
|`--log-level <log-level>`|Level of messages to log|`Trace`, `Debug`, `Information`, `Warning`, `Error`, `Critical`, `None`|`Information`|
|`--no-color`|Disable colored output|n/a|n/a|
|`--output <format>`|Output format|`Text`, `Json`|`Text`|
|`--pid <pid>`|Show status of a specific Dev Proxy instance by process ID|integer|n/a|

## Remarks

When you run multiple Dev Proxy instances (using `--as-system-proxy false`), the `status` command lists all running instances. Use the `--pid` option to show the status of a specific instance.

The command uses the current user's instance credentials to check the API status. Its output includes each instance's API URL and API token. When the `CI` environment variable is set, Dev Proxy omits the token. Use [api token](api-token.md) to retrieve it explicitly.

With `--output json`, the command returns one result event per instance with the `pid`, `apiUrl`, `token`, `apiStatus`, `message`, `port`, `recording`, `asSystemProxy`, `configFile`, `logFile`, and `startedAt` properties. When no matching instance is running, it returns `running: false`.

## See also

- [stop](stop-command.md)
- [api token](api-token.md)
- [logs](logs-command.md)
- [Use the system proxy option](../how-to/use-system-proxy-option.md)
