---
title: config new
description: config new command reference
author: garrytrinder
ms.author: garrytrinder
ms.date: 09/14/2026
---

<!-- INTENT: Reference for devproxy config new command -->
<!-- COMMAND: devproxy config new -->

# config new

Create a new Dev Proxy configuration file from a template included with Dev Proxy. The command doesn't require an internet connection. If a file exists with the same name, the created file has a number appended to it, for example, `devproxyrc-2.json`, and increments with each new file created with the same name.

## Synopsis

```text
devproxy config new [name] [options]

Arguments:
  [name]                 Name of the configuration file

Options:
  --format <format>      Configuration format to use: json|yaml
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
| `<name>` | Name of the configuration file. | No | `devproxyrc.json`, or `devproxyrc.yaml` when you use `--format yaml` |

## Options

| Name | Description | Allowed values | Default |
| -- | -- | -- | -- |
| `--format <format>` | Configuration file format. When you omit this option and specify a file name, Dev Proxy infers the format from the `.yaml` or `.yml` file extension. For other extensions, it uses JSON. | `json`, `yaml` | `json` |
| `--log-level <loglevel>` | Level of messages to log | `trace`, `debug`, `information`, `warning`, `error` | `information` |
