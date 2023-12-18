---
title: Use preset configurations
description: How to choose a preset configuration
author: garrytrinder
ms.author: garrytrinder
ms.date: 12/18/2023
ms.topic: how-to
ms.service: microsoft-cloud-for-developers

categories:
  - developer-tools
products:
  - microsoft-365
  - microsoft-graph
  - sharepoint-online
  - m365
ms.custom:
  - fcp
  - team=cloud_advocates
  - tool=devproxy
---

# Use preset configurations

Using presets you can quickly configure Dev Proxy to work with common scenarios. A preset is a JSON file that contains the configuration for Dev Proxy. The file defines which plugins Dev Proxy uses and how they're configured.

We include several preset configurations with Dev Proxy. You can find them in the `presets` folder in the Dev Proxy installation directory. You can also create your own preset configurations.

> [!TIP]
> Check out [Dev Proxy presets](https://aka.ms/devproxy/samples) created by the community in the Sample Solution Gallery.

To use a preset, use the `--config-file` option and pass the path to the preset file:

```sh
devproxy --config-file presets/microsoft-graph-rate-limiting.json
```
