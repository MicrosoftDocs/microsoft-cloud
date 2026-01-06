---
title: config new
description: config new command reference
author: garrytrinder
ms.author: garrytrinder
ms.date: 01/03/2026
---

<!-- INTENT: Reference for devproxy config new command -->
<!-- COMMAND: devproxy config new -->

# config new

Create new Dev Proxy configuration file. If a file exists with the same name, the created file will have a number appended to it, for example, devproxyrc-2.json, and will increment with each new file created with the same name.

## Synopsis

```text
devproxy config new [name] [options]

Arguments:
  [name]                 Name of the config file (default: devproxyrc.json)

Options:
  --log-level <level>    Logging level: trace|debug|information|warning|error
  -h, --help             Show help
```

:::image type="content" source="../media/config-new-command.png" alt-text="Screenshot of a command prompt with the output of the config new command." lightbox="../media/config-new-command.png":::

## Usage

```console
devproxy config new <name>
```

## Arguments

| Name | Description | Required | Default |
| ---- | ----------- | :------: | :-----: |
| `<name>` | Name of the configuration file. | No | `devproxyrc.json` |

## Options

|Name|Description|Allowed values|Default|
|--|--|--|--|
|`--log-level <loglevel>`|Level of messages to log|`trace`, `debug`, `information`, `warning`, `error`| `information`|
