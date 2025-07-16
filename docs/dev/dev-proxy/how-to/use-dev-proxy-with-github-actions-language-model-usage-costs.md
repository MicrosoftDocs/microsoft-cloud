---
title: How to use Dev Proxy to track language model usage and costs with GitHub Action workflows.
description: How to use Dev Proxy Actions to track language model usage and costs with GitHub Action workflows.
author: garrytrinder
ms.author: wmastyka
ms.date: 07/15/2025
---

# How to use Dev Proxy to track language model usage and costs with GitHub Actions

To integrate Dev Proxy into your GitHub Actions workflows, use [Dev Proxy Actions](https://github.com/marketplace/actions/dev-proxy-actions).

## Configure Dev Proxy for tracking language model usage

To track language model usage, configure a Dev Proxy configuration file in your project with the [OpenAITelemetryPlugin](../technical-reference/openaitelemetryplugin.md). To generate a report with the total costs and usage details, include the [MarkdownReporter](../technical-reference/markdownreporter.md) plugin. Include the URL of the OpenAI compatible API that you want to track in the `urlsToWatch` section of the configuration file.

```jsonc
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v0.29.2/devproxyrc.schema.json",
  "plugins": [
    {
      "name": "OpenAITelemetryPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll"
    },
    {
      "name": "MarkdownReporter",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll"
    }
  ],
  "urlsToWatch": [
    // Azure OpenAI
    "https://*.openai.azure.com/*",
    // OpenAI
    "https://api.openai.com/*",
    // GitHub Models
    "https://models.github.com/*"
  ]
}
```

## Configure OpenAITelemetryPlugin to track language model usage costs

To track language model usage costs, add a config section for the OpenAITelemetryPlugin. Set the `includeCosts` property to `true` to enable cost tracking. Specify the path to a JSON file with the model prices in the `pricesFile` property.

```jsonc
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v0.29.2/devproxyrc.schema.json",
  "plugins": [
    {
      "name": "OpenAITelemetryPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll"
    },
    {
      "name": "MarkdownReporter",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll"
    },
  ],
  {
    "openAITelemetryPlugin": {
      "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v0.29.2/openaitelemetryplugin.schema.json",
      "includeCosts": true,
      "pricesFile": "prices.json"
  },
  "urlsToWatch": [
    // Azure OpenAI
    "https://*.openai.azure.com/*",
    // OpenAI
    "https://api.openai.com/*",
    // GitHub Models
    "https://models.github.com/*"
  ]
}
```

Create a prices file with the input and output costs (price per million tokens), for the models you use. The model name in the prices file should match the model name returned in the API responses.

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v0.29.2/openaitelemetryplugin.pricesfile.schema.json",
  "prices": {
    "gpt-4": {
      "input": 0.03,
      "output": 0.06
    }
  }
}
```

## Change the currency used in the usage report costs

To change the currency used in the usage report costs, set the `currency` property in the OpenAITelemetryPlugin configuration. The default value is `USD`.

```json
{
  "openAITelemetryPlugin": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v0.29.2/openaitelemetryplugin.schema.json",
    "includeCosts": true,
    "pricesFile": "prices.json",
    "currency": "EUR"
  }
}
```

## Change the name and environment in the usage report heading

By default, the format usedd to create the report heading of `LLM usage report for <application> in <environment>`. To change the name and environment values, set the `application` and `environment` properties in the OpenAITelemetryPlugin configuration. The default values are `default` and `development`, respectively.

```json
{
  "openAITelemetryPlugin": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v0.29.2/openaitelemetryplugin.schema.json",
    "application": "My application",
    "environment": "Staging"
  }
}
```

## Set up Dev Proxy in the GitHub Actions workflow

To install and start Dev Proxy, use the `setup` action. To start in recording mode to capture requests for the OpenAITelemetryPlugin to process, set `auto-record` input to `true`. To include the usage report in the workflow job summary, pass the `$GITHUB_STEP_SUMMARY` variable to the `report-job-summary` input.

```yaml
- name: Setup Dev Proxy
  uses: dev-proxy-tools/actions/setup@v1
  with:
    auto-record: true
    report-job-summary: $GITHUB_STEP_SUMMARY
```

## Trigger requests to record

To interact with your application and trigger requests that Dev Proxy can record, use an end-to-end testing framework like [Playwright](https://playwright.dev/). The setup action automatically sets the `http_proxy` and `https_proxy` environment variables, which route requests through Dev Proxy.

```yaml
- name: Setup Dev Proxy
  uses: dev-proxy-tools/actions/setup@v1
  with:
    auto-record: true
    report-job-summary: $GITHUB_STEP_SUMMARY

- name: Run Playwright tests
  run: npx playwright test
```

## Install Dev Proxy certificate in Chromium browsers

If you're using Chromium-based browsers on Linux runners to generate requests for Dev Proxy to record, you need to install the Dev Proxy certificate to avoid SSL errors. To install the certificate, use the `chromium-cert` action.

```yaml
- name: Setup Dev Proxy
  uses: dev-proxy-tools/actions/setup@v1
  with:
    auto-record: true
    report-job-summary: $GITHUB_STEP_SUMMARY

- name: Install Dev Proxy certificate for Chromium browsers
  uses: dev-proxy-tools/actions/chromium-cert@v1

- name: Run Playwright tests
  run: npx playwright test
```

## Upload the usage report to artifacts

To generate the usage report, use the `stop` action to manually stop Dev Proxy in your workflow. To upload the usage report as an artifact, use the `actions/upload-artifact` action.

```yaml
- name: Stop recording
  uses: dev-proxy-tools/actions/stop@v1

- name: Upload Dev Proxy reports
  uses: actions/upload-artifact@v4
  with:
    name: Reports
    path: ./*Reporter*
```
