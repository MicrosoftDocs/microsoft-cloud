---
title: Use Dev Proxy with Azure Pipelines
description: How to use Dev Proxy with Azure Pipelines.
author: waldekmastykarz
ms.author: wmastyka
ms.date: 06/28/2024
---

# Use Dev Proxy with Azure Pipelines

Using Dev Proxy with Azure Pipelines is a great way to test your applications in a controlled environment. Following these steps, you can use Dev Proxy with your Azure Pipelines.

> [!NOTE]
> In this article, we use an Ubuntu agent for Azure Pipelines.

## Install Dev Proxy and cache it

Start, by installing Dev Proxy on the agent, but do it only if it isn't already installed. To install and cache Dev Proxy, add the following steps to your pipeline file:

```yaml
variables:
- name: DEV_PROXY_VERSION
  value: v0.29.0
- name: DEV_PROXY_CACHE_RESTORED
  value: 'false'

steps:
- task: Cache@2
  inputs:
    key: '"dev-proxy-$(DEV_PROXY_VERSION)"'
    path: ./devproxy
    cacheHitVar: DEV_PROXY_CACHE_RESTORED
  displayName: Cache Dev Proxy

- script: bash -c "$(curl -sL https://aka.ms/devproxy/setup.sh)" $DEV_PROXY_VERSION
  displayName: 'Install Dev Proxy'
  condition: ne(variables.DEV_PROXY_CACHE_RESTORED, 'true')
```

## Run the Dev Proxy

When you run Dev Proxy in a CI/CD pipeline, you need to [start it from a script](./use-dev-proxy-in-ci-cd-overview.md), so that you can close it gracefully. When you have your script ready, invoke it in your pipeline file:

```yaml
- script: bash ./run.sh
  displayName: 'Run Dev Proxy'
```

## Upload Dev Proxy logs as artifacts

When you run Dev Proxy in a CI/CD pipeline, you might want to analyze the logs later. To keep Dev Proxy logs, you can upload them as artifacts:

```yaml
- task: PublishPipelineArtifact@1
  displayName: Upload Dev Proxy logs
  inputs:
    targetPath: $(LOG_FILE)
    artifact: $(LOG_FILE)
```

## Upload Dev Proxy reports

If you're using Dev Proxy to analyze the requests, you might want to upload the reports as artifacts as well:

```yaml
- script: |
    mkdir -p $(Build.ArtifactStagingDirectory)/Reports
    for file in *Reporter*; do
      if [ -f "$file" ]; then
        cp "$file" $(Build.ArtifactStagingDirectory)/Reports
      fi
    done
  displayName: 'Copy reports to artifact directory'

- task: PublishPipelineArtifact@1
  displayName: Upload Dev Proxy reports
  inputs:
    targetPath: '$(Build.ArtifactStagingDirectory)/Reports'
    artifact: 'Reports'
```

## Example pipeline file

Here's an example of a pipeline file that installs Dev Proxy, runs it, and uploads the logs and reports as artifacts:

```yaml
trigger:
- main
- dev

pool:
  vmImage: ubuntu-latest

variables:
- name: LOG_FILE
  value: devproxy.log
- name: DEV_PROXY_VERSION
  value: v0.29.0
- name: DEV_PROXY_CACHE_RESTORED
  value: 'false'
- name: PLAYWRIGHT_CACHE_RESTORED
  value: 'false'

steps:

- task: UseNode@1
  inputs:
    version: '20.x'
  displayName: 'Install Node.js'

- script: npm ci
  displayName: 'Install dependencies'

#################################
# Cache + install of Playwright #
#################################
- script: |
    PLAYWRIGHT_VERSION=$(npm ls @playwright/test | grep @playwright | sed 's/.*@//')
    echo "Playwright's Version: $PLAYWRIGHT_VERSION"
    echo "##vso[task.setvariable variable=PLAYWRIGHT_VERSION]$PLAYWRIGHT_VERSION"
  displayName: Store Playwright's Version

- task: Cache@2
  inputs:
    key: '"playwright-ubuntu-$(PLAYWRIGHT_VERSION)"'
    path: '$(HOME)/.cache/ms-playwright'
    cacheHitVar: PLAYWRIGHT_CACHE_RESTORED
  displayName: Cache Playwright Browsers for Playwright's Version

- script: npx playwright install --with-deps
  condition: ne(variables['PLAYWRIGHT_CACHE_RESTORED'], 'true')
  displayName: 'Install Playwright Browsers'

################################
# Cache + install of Dev Proxy #
################################
- task: Cache@2
  inputs:
    key: '"dev-proxy-$(DEV_PROXY_VERSION)"'
    path: ./devproxy
    cacheHitVar: DEV_PROXY_CACHE_RESTORED
  displayName: Cache Dev Proxy

- script: bash -c "$(curl -sL https://aka.ms/devproxy/setup.sh)" $DEV_PROXY_VERSION
  displayName: 'Install Dev Proxy'
  condition: ne(variables.DEV_PROXY_CACHE_RESTORED, 'true')

- script: bash ./run.sh
  displayName: 'Run Dev Proxy'

- task: PublishPipelineArtifact@1
  displayName: Upload Dev Proxy logs
  inputs:
    targetPath: $(LOG_FILE)
    artifact: $(LOG_FILE)

- script: |
    mkdir -p $(Build.ArtifactStagingDirectory)/Reports
    for file in *Reporter*; do
      if [ -f "$file" ]; then
        cp "$file" $(Build.ArtifactStagingDirectory)/Reports
      fi
    done
  displayName: 'Copy reports to artifact directory'

- task: PublishPipelineArtifact@1
  displayName: Upload Dev Proxy reports
  inputs:
    targetPath: '$(Build.ArtifactStagingDirectory)/Reports'
    artifact: 'Reports'
```
