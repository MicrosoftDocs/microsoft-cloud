---
title: Use Dev Proxy with GitHub Actions
description: How to use Dev Proxy with GitHub Actions
author: estruyf
ms.author: wmastyka
ms.date: 07/09/2025
---

# Use Dev Proxy with GitHub Actions

To integrate Dev Proxy into your GitHub Actions workflows, use [Dev Proxy Actions](https://github.com/marketplace/actions/dev-proxy-actions).

## Set up Dev Proxy in your GitHub Actions workflow

To install and start Dev Proxy, use the `setup` action.

```yaml
- name: Setup Dev Proxy
  uses: dev-proxy-tools/actions/setup@v1
```

## Install and start Dev Proxy in recording mode

To start Dev Proxy in recording mode, set the `auto-record` input to `true`. This configuration allows Dev Proxy to capture requests and responses for further processing.

```yaml
- name: Start Dev Proxy
  uses: dev-proxy-tools/actions/start@v1
  with:
    auto-record: true
```

## Install and start Dev Proxy using a specific configuration file

By default, the default Dev Proxy configuration file is used, `devproxyrc.json`. To use a specific Dev Proxy configuration file, set the `config-file` input to the path of your configuration file.

```yaml
- name: Start Dev Proxy with config
  uses: dev-proxy-tools/actions/start@v1
  with:
    config-file: .devproxy/my-config.json
```

## Install and start Dev Proxy with a custom log file

By default, Dev Proxy output is logged to devproxy.log file in the working directory. To specify a custom log file, set the `log-file` input.

```yaml
- name: Start Dev Proxy with custom log file
  uses: dev-proxy-tools/actions/start@v1
  with:
    log-file: .devproxy/custom-devproxy.log
```

## Install a specific version of Dev Proxy

By default, the `setup` action installs the latest version of Dev Proxy. If you want to install a specific version, you can specify the `version` input.

```yaml
- name: Setup Dev Proxy with specific version
  uses: dev-proxy-tools/actions/setup@v1
  with:
    version: 0.29.2
```

## Install Dev Proxy only

To install Dev Proxy without starting it, set the `auto-start` input to `false`.

```yaml
- name: Install Dev Proxy
  uses: dev-proxy-tools/actions/setup@v1
  with:
    auto-start: false
```

## Start Dev Proxy manually

To start Dev Proxy manually after installation, use the `start` action.

```yaml
- name: Start Dev Proxy manually
  uses: dev-proxy-tools/actions/start@v1
```

The `start` action behaves similarly to the `setup` action, but it can't be used to install Dev Proxy. It shares the same inputs (except for `version`) and outputs as the `setup` action.

## Disable automatic stopping of Dev Proxy

By default, the `setup` and `start` actions stop Dev Proxy automatically after the job complete. To disable the automatic stopping of Dev Proxy after the job completes, set the `auto-stop` input to `false`.

```yaml
- name: Setup Dev Proxy without auto-stop
  uses: dev-proxy-tools/actions/setup@v1
  with:
    auto-stop: false
```

## Stop Dev Proxy manually

If you want to stop Dev Proxy manually, use the `stop` action. This action is useful if you want to generate reports and upload them as artifacts, or run Dev Proxy with a different configuration.

```yaml
- name: Stop Dev Proxy manually
  uses: dev-proxy-tools/actions/stop@v1

- name: Upload Dev Proxy reports
  uses: actions/upload-artifact@v4
  with:
    name: Reports
    path: ./*Reporter*
```

## Start recording manually

To start recording manually, use the `start` action with the `auto-record` input set to `true`.

```yaml
- name: Start Dev Proxy in recording mode
  uses: dev-proxy-tools/actions/record-start@v1
```

## Stop recording manually

To stop recording manually, use the `record-stop` action.

```yaml
- name: Stop recording
  uses: dev-proxy-tools/actions/record-stop@v1
```

## Get the URL of the running Dev Proxy instance

To get the URL of the running Dev Proxy instance, use the `proxy-url` output from the `setup` or `start` action. Use `steps.<step_id>.outputs.proxy-url` syntax, where `<step_id>` is the ID of the step that runs the action.

```yaml
- name: Setup Dev Proxy
  id: setup-devproxy
  uses: dev-proxy-tools/actions/setup@v1

- name: Get Dev Proxy URL
  run: echo "Dev Proxy URL: ${{ steps.setup-devproxy.outputs.proxy-url }}"
```

## Get the URL of the Dev Proxy API

To get the URL of the Dev Proxy API, use the `api-url` output from the `setup` or `start` action. Use `steps.<step_id>.outputs.api-url` syntax, where `<step_id>` is the ID of the step that runs the action.

```yaml
- name: Setup Dev Proxy
  id: setup-devproxy
  uses: dev-proxy-tools/actions/setup@v1

- name: Get Dev Proxy API URL
  run: echo "Dev Proxy API URL: ${{ steps.setup-devproxy.outputs.api-url }}"
```
