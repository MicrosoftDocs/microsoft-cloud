---
title: config get
description: config get command reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 10/03/2026
---

<!-- INTENT: Reference for devproxy config get command -->
<!-- COMMAND: devproxy config get -->

# config get

Download a preset or sample from the [Dev Proxy samples gallery](https://aka.ms/devproxy/samples). A preset is a ready-made Dev Proxy config for a specific API or scenario, so you download it with `devproxy config get`.

## Synopsis

```text
devproxy config get <config-id> [options]

Arguments:
  <config-id>            ID of the config to download (required)

Options:
  --log-level <level>    Logging level: trace|debug|information|warning|error
  -h, --help             Show help
```

:::image type="content" source="../media/config-get-command.png" alt-text="Screenshot of a command prompt with the output of the config get command." lightbox="../media/config-get-command.png":::

## Usage

```console
devproxy config get <config-id>
```

## Arguments

| Name | Description | Required | Default |
| ---- | ----------- | :------: | :-----: |
| `<config-id>` | The ID of the preset or sample to download. | Yes | None |

> [!TIP]
> Each preset and sample lists its ID in the details section on its page in the Dev Proxy samples gallery.

## Options

|Name|Description|Allowed values|Default|
|--|--|--|--|
|`--log-level <loglevel>`|Level of messages to log|`trace`, `debug`, `information`, `warning`, `error`| `information`|

## Remarks

Dev Proxy stores downloaded presets and samples in the `~dataFolder/configs/<config-id>` folder. Upgrading Dev Proxy doesn't affect them.

Presets keep their config in a `.devproxy` subfolder. To start Dev Proxy with a downloaded preset, run:

```console
devproxy --config-file "~dataFolder/configs/<config-id>/.devproxy/devproxyrc.json"
```
