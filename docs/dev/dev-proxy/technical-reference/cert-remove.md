---
title: cert remove
description: cert remove command reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/03/2026
---

<!-- INTENT: Reference for devproxy cert remove command -->
<!-- COMMAND: devproxy cert remove -->

# cert remove

Removes the certificate that Dev Proxy uses for decrypting SSL traffic from Root Store.

## Synopsis

```text
devproxy cert remove [options]

Options:
  -f, --force            Force certificate removal without confirmation
  --log-level <level>    Logging level: trace|debug|information|warning|error
  -h, --help             Show help
```

## Usage

```console
devproxy cert remove
```

## Arguments

None

## Options

|Name|Description|Allowed values|Default|
|--|--|--|--|
|`-f, --force`|Force the root certificate removal| | |
|`--log-level <loglevel>`|Level of messages to log|`trace`, `debug`, `information`, `warning`, `error`| `information`|
