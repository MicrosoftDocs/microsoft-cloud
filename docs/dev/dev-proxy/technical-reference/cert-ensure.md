---
title: cert ensure
description: cert ensure command reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/03/2026
---

<!-- INTENT: Reference for devproxy cert ensure command -->
<!-- COMMAND: devproxy cert ensure -->

# cert ensure

Ensures that the certificate that Dev Proxy uses for decrypting SSL traffic is set up. Creates the certificate if it doesn't exist. Also, makes the certificate trusted.

## Synopsis

```text
devproxy cert ensure [options]

Options:
  --log-level <level>    Logging level: trace|debug|information|warning|error
  -h, --help             Show help
```

## Usage

```console
devproxy cert ensure
```

## Arguments

None

## Options

|Name|Description|Allowed values|Default|
|--|--|--|--|
|`--log-level <loglevel>`|Level of messages to log|`trace`, `debug`, `information`, `warning`, `error`| `information`|
