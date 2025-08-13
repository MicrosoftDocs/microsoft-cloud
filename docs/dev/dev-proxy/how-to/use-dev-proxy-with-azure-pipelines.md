---
title: Use Dev Proxy with Azure Pipelines
description: How to use Dev Proxy with Azure Pipelines.
author: waldekmastykarz
ms.author: wmastyka
ms.date: 07/09/2025
---

# Use Dev Proxy with Azure Pipelines

Using Dev Proxy with Azure Pipelines is a great way to test your applications in a controlled environment. Following these steps, you can use Dev Proxy with your Azure Pipelines.

> [!NOTE]
> In this article, we use an Ubuntu agent for Azure Pipelines.

## Install Dev Proxy and cache it

Start by installing Dev Proxy on the agent using a script task to install Dev Proxy.

```yaml
- script: bash -c "$(curl -sL https://aka.ms/devproxy/setup.sh)"
  displayName: 'Install Dev Proxy'
```

By default, this installs the latest version of Dev Proxy. If you want to install a specific version, you can specify it by passing a version at the end of the script:

```yaml
- script: bash -c "$(curl -sL https://aka.ms/devproxy/setup.sh)" v1.0.0
  displayName: 'Install Dev Proxy v1.0.0'
```

A recommended practice is to cache the Dev Proxy installation files to speed up subsequent runs. You can use the `Cache` task for this purpose. Here's how you can do it:

```yaml
variables:
- name: DEV_PROXY_VERSION
  value: v1.0.0
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

## Start Dev Proxy

When you start Dev Proxy in a CI/CD pipeline, you can [start it from a script](./use-dev-proxy-in-ci-cd-overview.md#example-start-script), or include the script inline. When you have your script ready, invoke it in your pipeline file:

```yaml
- script: bash ./run.sh
  displayName: 'Start Dev Proxy'
```

## Control Dev Proxy

To control Dev Proxy, you can use the [Dev Proxy API](../technical-reference/proxy-api.md). For example, to start recording requests, you can send a POST request to the `/proxy` endpoint:

```yaml
- script: |
    curl -X POST http://localhost:8897/proxy -H "Content-Type: application/json" -d '{"recording": true}'
  displayName: 'Start recording'
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

Here's a simple example of how to use Dev Proxy in an Azure Pipeline. This workflow installs Dev Proxy, starts it, sends a request through it using curl, and then shows the logs.

```yaml
trigger:
- main
- dev

pool:
  vmImage: ubuntu-latest

variables:
- name: DEV_PROXY_VERSION
  value: v1.0.0

steps:
- script: bash -c "$(curl -sL https://aka.ms/devproxy/setup.sh)" $DEV_PROXY_VERSION
  displayName: 'Install Dev Proxy'

- script: bash ./start.sh
  displayName: 'Start Dev Proxy'

- script: |
     curl -ikx http://127.0.0.1:8000 https://jsonplaceholder.typicode.com/posts
  displayName: 'Send request'

- script: |
    curl -X POST http://localhost:8897/proxy/stop
  displayName: 'Stop Dev Proxy'

- script: |
    echo "Dev Proxy logs:"
    cat devproxy.log
  displayName: 'Show Dev Proxy logs'
```
