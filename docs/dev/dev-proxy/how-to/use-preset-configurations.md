---
title: Use preset configurations
description: How to choose a preset configuration
author: garrytrinder
ms.author: garrytrinder
ms.date: 01/03/2026
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

Using presets you can quickly configure Dev Proxy to work with common scenarios. A preset is a JSON file that contains the configuration for Dev Proxy. The file defines which plugins Dev Proxy uses and how they're configured.

We include several preset configurations with Dev Proxy. You can find them in the `config` folder in the Dev Proxy installation directory. You can also create your own preset configurations.

> [!TIP]
> Check out [Dev Proxy presets](https://aka.ms/devproxy/samples) created by the community in the Sample Solution Gallery.

To use a preset, use the `--config-file` option and pass the path to the preset file:

```console
devproxy --config-file config/microsoft-graph-rate-limiting.json
```

## See also

- [Configure Dev Proxy](../get-started/configure.md)
- [Change the mocks file](./change-mocks-file.md)
- [Dev Proxy presets](https://aka.ms/devproxy/samples)
