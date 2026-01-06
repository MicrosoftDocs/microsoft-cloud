---
title: outdated
description: outdated command reference
author: garrytrinder
ms.author: garrytrinder
ms.date: 01/03/2026
---

<!-- INTENT: Reference for devproxy outdated command -->
<!-- COMMAND: devproxy outdated -->

# outdated

Checks if the installed version is outdated and returns the version to upgrade to. The `newVersionNotification` property `devproxyrc.json` file determines the version check behavior.

## Synopsis

```text
devproxy outdated [options]

Options:
  --short                Return version only (no additional text)
  --log-level <level>    Logging level: trace|debug|information|warning|error
  -h, --help             Show help
```

## Usage

```console
devproxy outdated
```

## Arguments

None

## Options

|Name|Description|Allowed values|Default|
|--|--|--|--|
|`--log-level <loglevel>`|Level of messages to log|`trace`, `debug`, `information`, `warning`, `error`| `information`|
| `--short` | Return version only | N/A | N/A |
