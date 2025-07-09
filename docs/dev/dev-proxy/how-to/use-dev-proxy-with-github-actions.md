---
title: Use Dev Proxy with GitHub Actions
description: How to use Dev Proxy with GitHub Actions
author: estruyf
ms.author: wmastyka
ms.date: 07/09/2025
---

# Use Dev Proxy with GitHub Actions

Using Dev Proxy with GitHub Actions is a great way to test your applications in a controlled environment. We [published a collection of GitHub Actions](https://github.com/marketplace/actions/dev-proxy-actions), which makes it easy to integrate Dev Proxy into your GitHub Action workflows.

> [!IMPORTANT]
> Dev Proxy Actions support Linux based runners only.

## Actions

The following actions are available for use in your GitHub Actions workflows:

- `dev-proxy-tools/actions/install`
- `dev-proxy-tools/actions/start`
- `dev-proxy-tools/actions/stop`
- `dev-proxy-tools/actions/record-start`
- `dev-proxy-tools/actions/record-stop`

## Install Dev Proxy

Start by installing Dev Proxy on the runner using the `dev-proxy-tools/actions/install` action.

```yaml
- name: Install Dev Proxy
  uses: dev-proxy-tools/actions/install@v1
```

By default, this action installs the latest version of Dev Proxy. If you want to install a specific version, you can specify it using the `version` input:

```yaml
- name: Install Dev Proxy
  uses: dev-proxy-tools/actions/install@v1
  with:
    version: v0.29.2
```

A recommended practice is to cache the Dev Proxy installation files to speed up subsequent runs. You can use the `actions/cache` action for this purpose. Here's how you can do it:

```yaml
- name: Cache Dev Proxy
  id: cache-devproxy
  uses: actions/cache@v4
  with:
    path: ./devproxy
    key: devproxy-linux-v0.29.2

- name: Install Dev Proxy
  if: steps.cache-devproxy.outputs.cache-hit != 'true'
  uses: dev-proxy-tools/actions/install@v1
  with:
    version: v0.29.2
```

## Start Dev Proxy

After installing Dev Proxy, you can start Dev Proxy in the background using the `dev-proxy-tools/actions/start` action.

```yaml
- name: Start Dev Proxy
  uses: dev-proxy-tools/actions/start@v1
```

By default, the action will:

- Start Dev Proxy with the default configuration file
- Output logs to `devproxy.log`
- Install and trust Dev Proxy's root certificate on the runner
- Set `https_proxy` and `http_proxy` environment variables to `http://127.0.0.1:8080` to route requests through Dev Proxy
- Registers a workflow post step to stop Dev Proxy and clear the `https_proxy` and `http_proxy` environment variables at the end of the workflow

You can customize the action by providing a path to a configuration file, a log file, and set whether you want to register the post step to stop Dev Proxy automatically. Here's an example:

```yaml
- name: Start Dev Proxy
  uses: dev-proxy-tools/actions/start@v1
  with:
    config-file: /path/to/your/config.json
    log-file: /path/to/your/logfile.log
    auto-stop: false
```

## Stop Dev Proxy

To stop Dev Proxy manually, you can use the `dev-proxy-tools/actions/stop` action. This action stops the Dev Proxy process and clears the `https_proxy` and `http_proxy` environment variables. It's useful if you want to run some steps without Dev Proxy running, or if you want to stop it before the end of the workflow.

```yaml
- name: Stop Dev Proxy
  uses: dev-proxy-tools/actions/stop@v1
```

## Recording

Recording mode is a powerful feature of Dev Proxy that allows you to capture Dev Proxy activity. Plugins that can generate reports require recording mode to be enabled.

To start recording, you can use the `dev-proxy-tools/actions/record-start` action.

```yaml
- name: Start recording
  uses: dev-proxy-tools/actions/record-start@v1
```

To stop recording, you can use the `dev-proxy-tools/actions/record-stop` action.

```yaml
- name: Stop recording
  uses: dev-proxy-tools/actions/record-stop@v1
```

## Upload Dev Proxy logs as artifacts

At the end of your workflow, you might want to upload the Dev Proxy logs as artifacts. Use the `actions/upload-artifact` action to upload the logs.

```yaml
- name: Upload Dev Proxy logs
  uses: actions/upload-artifact@v4
  with:
    name: /path/to/your/logfile.log
    path: /path/to/your/logfile.log
```

## Upload Dev Proxy reports

If you're using plugins that can generate reports, you can upload the generated reports as artifacts.

```yaml
- name: Upload Dev Proxy reports
  uses: actions/upload-artifact@v4
  with:
    name: Reports
    path: ./*Reporter*
```

## Write job summary

To make it easier to understand the results of your workflow, you can write a summary at the end of your job. Typically, you use the `MarkdownReporter` for generating job summaries, because it produces output in Markdown format:

```yaml
- name: Write summary
  run: |
    cat YourPlugin_MarkdownReporter.md >> $GITHUB_STEP_SUMMARY
```

## Example workflows

The following examples demonstrate how to use Dev Proxy in GitHub Actions workflows. You can adapt these examples to fit your specific use case.

## Simple example

Here's a simple example of how to use Dev Proxy in a GitHub Actions workflow. This workflow installs Dev Proxy, starts it, sends a request through it using curl, and then shows the logs.

```yaml
name: Example Dev Proxy workflow
on:
  workflow_dispatch:

jobs:
  example-dev-proxy:
    name: Example Dev Proxy Job
    runs-on: ubuntu-latest
    steps:
      - name: Intall Dev Proxy
        id: install-latest
        uses: dev-proxy-tools/actions/install@v1

      - name: Start Dev Proxy
        id: start-devproxy
        uses: dev-proxy-tools/actions/start@v1

      - name: Send request
        id: send-request
        run: |
          curl -ikx http://127.0.0.1:8000 https://jsonplaceholder.typicode.com/posts

      - name: Show logs
        run: |
          echo "Dev Proxy logs:"
          cat devproxy.log
  ```

## Playwright example

If you're using Playwright for testing, you can integrate Dev Proxy into your Playwright tests.

Here's an example of a complete workflow file that uses Dev Proxy and Playwright (based on original example by [Elio Struyf](https://www.eliostruyf.com/playwright-microsoft-dev-proxy-github-actions/)):

```yaml
name: Test app using Dev Proxy

on:
  push:
    branches:
      - main
      - dev
  workflow_dispatch:

jobs:
  test:
    name: Test app using Dev Proxy
    timeout-minutes: 60
    runs-on: ubuntu-latest
    env:
      LOG_FILE: devproxy.log
      DEVPROXY_VERSION: v0.29.2
    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-node@v4
        with:
          cache: "npm"

      - name: Install dependencies
        run: npm ci

      #################################
      # Cache + install of Playwright #
      #################################
      - name: Store Playwright's Version
        run: |
          PLAYWRIGHT_VERSION=$(npm ls @playwright/test | grep @playwright | sed 's/.*@//')
          echo "Playwright's Version: $PLAYWRIGHT_VERSION"
          echo "PLAYWRIGHT_VERSION=$PLAYWRIGHT_VERSION" >> $GITHUB_ENV          

      - name: Cache Playwright Browsers for Playwright's Version
        id: cache-playwright
        uses: actions/cache@v4
        with:
          path: ~/.cache/ms-playwright
          key: playwright-ubuntu-${{ env.PLAYWRIGHT_VERSION }}

      - name: Install Playwright Browsers
        if: steps.cache-playwright.outputs.cache-hit != 'true'
        run: npx playwright install --with-deps

      ################################
      # Cache + install of Dev Proxy #
      ################################
      - name: Cache Dev Proxy
        id: cache-devproxy
        uses: actions/cache@v4
        with:
          path: ./devproxy
          key: devproxy-ubuntu-${{ env.DEVPROXY_VERSION }}

      - name: Install Dev Proxy
        if: steps.cache-devproxy.outputs.cache-hit != 'true'
        uses: dev-proxy-tools/actions/install@v1
        with:
          version: ${{ env.DEVPROXY_VERSION }}
      
      - name: Start Dev Proxy
        uses: dev-proxy-tools/actions/start@v1
        with:
          log-file: ${{ env.LOG_FILE }}

      - name: Start recording
        uses: dev-proxy-tools/actions/record-start@v1

      - name: Run tests
        run: npm test

      - name: Stop recording
        uses: dev-proxy-tools/actions/record-stop@v1

      - name: Upload Dev Proxy logs
        uses: actions/upload-artifact@v4
        with:
          name: ${{ env.LOG_FILE }}
          path: ${{ env.LOG_FILE }}
      
      # only when using a reporting plugin with the Markdown reporter
      - name: Upload Dev Proxy reports
        uses: actions/upload-artifact@v4
        with:
          name: Reports
          path: ./*Reporter*
      
      # only when using a reporting plugin with the Markdown reporter
      - name: Write summary
        run: |
          cat SomePlugin_MarkdownReporter.md >> $GITHUB_STEP_SUMMARY
```
