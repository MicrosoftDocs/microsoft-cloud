---
title: config open
description: config open command reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/03/2026
---

<!-- INTENT: Reference for devproxy config open command -->
<!-- COMMAND: devproxy config open -->

# config open

Opens Dev Proxy config file (devproxyrc.json) in the default editor associated with JSON files. If there's a devproxyrc.json file in the current directory, the command opens it in the editor. Otherwise, the command opens the global Dev Proxy config file located in the installation directory.

## Synopsis

```text
devproxy config open [options]

Options:
  --log-level <level>    Logging level: trace|debug|information|warning|error
  -h, --help             Show help
```

## Usage

```console
devproxy config open
```

## Arguments

None

## Options

|Name|Description|Allowed values|Default|
|--|--|--|--|
|`--log-level <loglevel>`|Level of messages to log|`trace`, `debug`, `information`, `warning`, `error`| `information`|
