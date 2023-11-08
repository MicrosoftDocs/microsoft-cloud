---
title: Get started
description: Get started with Microsoft 365 Developer Proxy
author: garrytrinder
ms.author: garrytrinder
ms.contributors: garrytrinder
ms.date: 11/03/2023
ms.topic: conceptual
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
---

We provide many preset configurations that can be used to recreate common scenarios against known APIs.

Presets make it easier to load a known configuration without having to configure a settings file.

The configurations are stored in the presets folder in the proxy installation directory.

To use a preset, use the `--config-file` option and pass the path to the configuration file:

```sh
m365proxy --config-file presets/microsoft-graph-rate-limiting.json
```
