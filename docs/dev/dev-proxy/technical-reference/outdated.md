---
title: outdated
description: outdated command reference
author: garrytrinder
ms.author: garrytrinder
ms.date: 03/31/2025
---

# outdated

Checks if the installed version is outdated and returns the version to upgrade to. The `newVersionNotification` property `devproxyrc.json` file determines the version check behavior.

## Usage

```console
devproxy outdated
```

## Arguments

None

## Options

|Name|Description|Allowed values|Default|
|--|--|--|--|--|
|`--log-level <loglevel>`|Level of messages to log|`trace`, `debug`, `information`, `warning`, `error`| `information`|
| `--short` | Return version only | N/A | N/A |
