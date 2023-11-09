---
title: Change console output to use icons
description: How to change console output to use icons
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

# Change console output to use icons

By default, the proxy console output uses [text labels](./Console-output-text-labels.md).

To change the label style, update the `labelMode` property value in the [m365proxyrc.json](../technical-reference//m365proxyrc.md) file stored in your installation directory.

## ASCII icons

To use ASCII icons, use:

```json
"labelMode": "icons",
```

## Nerdfont icons

To use NerdFont icons, first ensure that you have a compatible variable-width [font installed](https://www.nerdfonts.com/font-downloads), then use:

```json
"labelMode": "nerdFont"
```
