---
title: cert remove
description: cert remove command reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 06/13/2025
---

# cert remove

Removes the certificate that Dev Proxy uses for decrypting SSL traffic from Root Store.

## Usage

```console
devproxy cert remove
```

## Arguments

None

## Options

|Name|Description|Allowed values|Default|
|--|--|--|--|--|
|`-f, --force`|Force the root certificate removal| | |
|`--log-level <loglevel>`|Level of messages to log|`trace`, `debug`, `information`, `warning`, `error`| `information`|
