---
title: Use Dev Proxy with GitHub Actions
description: How to use Dev Proxy with GitHub Actions
author: estruyf
ms.author: wmastyka
ms.date: 05/27/2024
---

# Use Dev Proxy with GitHub Actions

Using Dev Proxy with GitHub Actions is a great way to test your applications in a controlled environment. If you want to use Dev Proxy with your GitHub Actions workflow, you need to include a couple of steps in your workflow file to be able to trust the Dev Proxy certificate.

> [!NOTE]
> In this example, we will make use of an Ubuntu runner for GitHub Actions.

## Install Dev Proxy and cache it

Start, by installing Dev Proxy on the runner, but do it only if it isn't already installed. To install and cache Dev Proxy, add the following steps to your workflow file:

```yaml
- name: Cache Dev Proxy
  id: cache-devproxy
  uses: actions/cache@v4
  with:
    path: ./devproxy
    key: devproxy-linux-v0.18.0

- name: Install Dev Proxy
  if: steps.cache-devproxy.outputs.cache-hit != 'true'
  run: bash -c "$(curl -sL https://aka.ms/devproxy/setup.sh)"
```

## Run the Dev Proxy

When you run Dev Proxy in a CI/CD pipeline, you need to [start it from a script](./use-dev-proxy-in-ci-cd-overview.md), so that you can close it gracefully. When you have your script ready, invoke it in your workflow file:

```yaml
- name: Run Dev Proxy
  run: /bin/bash run-dev-proxy.sh
```

## Upload Dev Proxy logs as artifacts

When you run Dev Proxy in a CI/CD pipeline, you might want to analyze the logs later. To keep Dev Proxy logs, you can upload them as artifacts:

```yaml
- name: Upload Dev Proxy logs
  uses: actions/upload-artifact@v4
  with:
    name: ${{ env.LOG_FILE }}
    path: ${{ env.LOG_FILE }}
```

## Upload Dev Proxy reports

If you're using Dev Proxy to analyze the requests, you might want to upload the reports as artifacts as well:

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
