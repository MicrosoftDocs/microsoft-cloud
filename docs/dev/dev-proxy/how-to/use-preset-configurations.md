---
title: Use preset configurations
description: How to choose a preset configuration
author: garrytrinder
ms.author: garrytrinder
ms.date: 10/03/2026
---

<!-- INTENT: Use pre-built Dev Proxy configs -->
<!-- SOLUTION: Run devproxy preset get and apply -->
<!-- RESULT: Ready-to-use configuration for specific scenario -->
<!-- PLUGINS: various -->
<!-- JOB: configure-proxy -->
<!-- TIME: 3 minutes -->

# Use preset configurations

> **At a glance**  
> **Goal:** Use pre-built Dev Proxy configs  
> **Time:** 3 minutes  
> **Plugins:** Various  
> **Prerequisites:** [Set up Dev Proxy](../get-started/set-up.md)

Using presets you can quickly configure Dev Proxy to work with common scenarios. A preset is a ready-made Dev Proxy config: a JSON file that defines which plugins Dev Proxy uses and how they're configured. Your own settings live in your config, for example `devproxyrc.json`.

## Download a preset

The [Dev Proxy samples gallery](https://aka.ms/devproxy/samples) has presets for specific APIs, like GitHub, OpenAI, Anthropic, and Microsoft Graph. It also has samples: demo projects that show how to use Dev Proxy in a specific scenario.

To download a preset, use the [`config get`](../technical-reference/config-get.md) command with the preset's ID, and then start Dev Proxy with the preset:

```console
devproxy config get github-rate-limiting
devproxy --config-file "~dataFolder/configs/github-rate-limiting/.devproxy/devproxyrc.json"
```

## Use a preset included with Dev Proxy

We include several presets with Dev Proxy. You can find them in the `config` folder in the Dev Proxy installation directory.

To use a preset, use the `--config-file` option and pass the path to the preset file:

```console
devproxy --config-file config/microsoft-graph-rate-limiting.json
```

## See also

- [Configure Dev Proxy](../get-started/configure.md)
- [Change the mocks file](./change-mocks-file.md)
- [Dev Proxy samples gallery](https://aka.ms/devproxy/samples)
