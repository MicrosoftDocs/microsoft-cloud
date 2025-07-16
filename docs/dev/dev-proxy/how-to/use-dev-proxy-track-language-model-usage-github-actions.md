---
title: How to use Dev Proxy to track language model usage in GitHub Actions workflows
description: Use Dev Proxy Actions to monitor language model usage and costs in your GitHub workflows
author: garrytrinder
ms.author: wmastyka
ms.date: 07/15/2025
---

# How to use Dev Proxy to track language model usage in GitHub Actions workflows

To integrate Dev Proxy into your GitHub workflows, use [Dev Proxy Actions](https://github.com/marketplace/actions/dev-proxy-actions).

## Configure Dev Proxy for tracking language model usage

To track language model usage and costs, configure a Dev Proxy configuration file in your project with the [OpenAITelemetryPlugin](../technical-reference/openaitelemetryplugin.md). To generate a report with the total costs and usage details, include the [MarkdownReporter](../technical-reference/markdownreporter.md) plugin.

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v0.29.2/devproxyrc.schema.json",
  "plugins": [
    {
      "name": "OpenAITelemetryPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "openAITelemetryPlugin"
    },
    {
      "name": "MarkdownReporter",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll"
    }
  ],
  "urlsToWatch": [
    "https://*.openai.azure.com/*",
    "https://api.openai.com/*",
    "https://models.github.com/*"
  ],
  "openAITelemetryPlugin": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v0.29.2/openaitelemetryplugin.schema.json",
    "application": "My app",
    "currency": "USD",
    "includeCosts": true,
    "pricesFile": "prices.json"
  }
}
```

Create a prices file with the input and output costs for the models you use. The model name in the prices file should match the model name returned in the API responses.

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

## Set up Dev Proxy in your GitHub Actions workflow

To install and start Dev Proxy, use the `setup` action. To start in recording mode to capture requests for the OpenAITelemetryPlugin to process, set `auto-record` input to `true`. To include the usage report in the workflow job summary, pass the `$GITHUB_STEP_SUMMARY` variable to the `report-job-summary` input.

```yaml
      - name: Setup Dev Proxy
        uses: dev-proxy-tools/actions/setup@v1
        with:
          auto-record: true
          report-job-summary: $GITHUB_STEP_SUMMARY
```

## Generate requests to record

To interact with your application and trigger requests that Dev Proxy can record, use an end-to-end testing framework like [Playwright](https://playwright.dev/). The setup action automatically sets the `http_proxy` and `https_proxy` environment variables, which route requests through Dev Proxy.

## Install Dev Proxy certificate in Chromium browsers

If you're using Chromium-based browsers on Linux runners to generate requests for Dev Proxy to record, you need to install the Dev Proxy certificate to avoid SSL errors. To install the certificate, use the `chromium-cert` action.

```yaml
      - name: Install Dev Proxy certificate for Chromium browsers
        uses: dev-proxy-tools/actions/chromium-cert@v1
```

## Upload the usage report to artifacts

To stop Dev Proxy manually, use the `stop` action. To upload the usage report as an artifact, use the `actions/upload-artifact` action.

```yaml
      - name: Stop recording
        uses: dev-proxy-tools/actions/record-stop@v1

      - name: Upload Dev Proxy reports
        uses: actions/upload-artifact@v4
        with:
          name: Reports
          path: ./*Reporter*
```
