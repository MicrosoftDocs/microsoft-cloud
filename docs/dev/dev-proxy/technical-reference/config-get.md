---
title: config get
description: config get command reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 03/31/2025
---

# config get

Download the specified config from the [Sample Solution Gallery](https://aka.ms/devproxy/samples).

:::image type="content" source="../media/config-get-command.png" alt-text="Screenshot of a command prompt with the output of the config get command." lightbox="../media/config-get-command.png":::

## Usage

```console
devproxy config get <config-id>
```

## Arguments

| Name | Description | Required | Default |
| ---- | ----------- | :------: | :-----: |
| `<config-id>` | The ID of the config to download. | Yes | None |

> [!TIP]
> Each sample lists its ID in the details section on the sample page in the Sample Solution Gallery.

## Options

|Name|Description|Allowed values|Default|
|--|--|--|--|
|`--log-level <loglevel>`|Level of messages to log|`trace`, `debug`, `information`, `warning`, `error`| `information`|
