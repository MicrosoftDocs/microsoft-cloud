---
title: config open
description: config open command reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 03/31/2025
---

# config open

Opens Dev Proxy config file (devproxyrc.json) in the default editor associated with JSON files. If there's a devproxyrc.json file in the current directory, the command opens it in the editor. Otherwise, the command opens the global Dev Proxy config file located in the installation directory.

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
