---
title: config validate
description: config validate command reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 02/27/2026
ms.topic: reference
---

<!-- INTENT: Reference for devproxy config validate command -->
<!-- COMMAND: devproxy config validate -->

# config validate

Validates a Dev Proxy configuration file without starting the proxy.

## Synopsis

```text
devproxy config validate [options]

Options:
  -c, --config-file <file>    Configuration file to validate (default: devproxyrc.json)
  --output <format>           Output format: text|json (default: text)
  -h, --help                  Show help
```

## Usage

```console
devproxy config validate
```

## Options

| Name | Description | Allowed values | Default |
| ---- | ----------- | -------------- | ------- |
| `-c, --config-file <configFile>` | The path to the configuration file to validate | Local file path | `devproxyrc.json` |
| `--output <format>` | Output format | `text`, `json` | `text` |
| `-?, -h, --help` | Show help and usage information | n/a | n/a |

## Examples

### Validate the default configuration file

```console
devproxy config validate
```

### Validate a specific configuration file

```console
devproxy config validate --config-file ./my-config.json
```

### Validate and get structured JSON output

```console
devproxy config validate --output json
```

## Exit codes

| Code | Meaning |
| ---- | ------- |
| `0` | Configuration is valid |
| `2` | Configuration is invalid or file not found |

## Remarks

The `config validate` command checks that:

- The configuration file exists and is valid JSON
- The configuration matches the specified `$schema`
- All referenced plugin paths exist and can be loaded
- URL patterns are syntactically correct

> [!NOTE]
> Schema validation only applies to JSON-based configuration files. YAML configuration files aren't validated against schemas at runtime.

Use this command in CI/CD pipelines to catch configuration errors before deploying.
