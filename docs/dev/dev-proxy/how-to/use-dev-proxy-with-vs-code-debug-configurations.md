---
title: Use Dev Proxy in Visual Studio Code debug configurations
description: How to use Dev Proxy in Visual Studio Code debug configurations
author: garrytrinder
ms.author: garrytrinder
ms.date: 01/03/2026
---

<!-- INTENT: Auto-start Dev Proxy when debugging in VS Code -->
<!-- SOLUTION: Add preLaunchTask to launch.json -->
<!-- RESULT: Dev Proxy starts automatically when debugging -->
<!-- PLUGINS: various -->
<!-- JOB: configure-proxy -->
<!-- TIME: 10 minutes -->

# Use Dev Proxy with Visual Studio Code debug configurations

> **At a glance**  
> **Goal:** Auto-start Dev Proxy when debugging in VS Code  
> **Time:** 10 minutes  
> **Plugins:** Various  
> **Prerequisites:** [Set up Dev Proxy](../get-started/set-up.md), VS Code, Dev Proxy Toolkit extension

Install [Dev Proxy Toolkit](https://marketplace.visualstudio.com/items?itemName=garrytrinder.dev-proxy-toolkit) from the extension marketplace. The extension provides tasks and watchers for Dev Proxy.

## Add Dev Proxy to your debug configuration

Add the `start` and `stop` tasks to your `tasks.json` file in your project.

> [!TIP]
> Use the `devproxy-task-start` and `devproxy-task-stop` snippets to quickly add Dev Proxy tasks to your `tasks.json` file.

**File:** .vscode/tasks.json

```json
{
    "version": "2.0.0",
    "tasks": [
        {
            "label": "devproxy-start",
            "type": "devproxy",
            "command": "start",
            "isBackground": true,
            "problemMatcher": "$devproxy-watch",
        },
        {
            "label": "devproxy-stop",
            "type": "devproxy",
            "command": "stop"
        }
    ]
}
```

Configure the `preLaunchTask` and `postDebugTask` properties with the task labels you defined in the `tasks.json` file. Dev Proxy starts before your application runs and stops after debugging is complete. The following example shows how to configure the `launch.json` file to start a debug session with Dev Proxy and a Node.js application.

**File:** .vscode/launch.json

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "type": "node",
            "request": "launch",
            "name": "Launch Program",
            "skipFiles": [
                "<node_internals>/**"
            ],
            "program": "${workspaceFolder}/index.mjs",
            "preLaunchTask": "devproxy-start",
            "postDebugTask": "devproxy-stop",
            "env": {
                "NODE_ENV": "development",
                "http_proxy": "http://127.0.0.1:8000",
                "https_proxy": "http://127.0.0.1:8000"
            }
        }
    ]
}
```

## Pass options to Dev Proxy

You can pass options to Dev Proxy by adding them to the `args` property on the `start` task in the `tasks.json` file. For example, to start Dev Proxy in recording mode pass the `--record` argument:

**File:** .vscode/tasks.json (task with args)

```json
{
    "label": "devproxy-start",
    "type": "devproxy",
    "command": "start",
    "args": [
        "--record"
    ],
    "isBackground": true,
    "problemMatcher": "$devproxy-watch"
}
```

## See also

- [Configure Dev Proxy](../get-started/configure.md)
- [Use preset configurations](./use-preset-configurations.md)
- [Dev Proxy Toolkit extension](https://marketplace.visualstudio.com/items?itemName=garrytrinder.dev-proxy-toolkit)
