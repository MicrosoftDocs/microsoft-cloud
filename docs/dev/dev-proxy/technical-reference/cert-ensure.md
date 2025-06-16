---
title: cert ensure
description: cert ensure command reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 03/31/2025
---

# cert ensure

Ensures that the certificate that Dev Proxy uses for decrypting SSL traffic is set up. Creates the certificate if it doesn't exist. Also, makes the certificate trusted.

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
