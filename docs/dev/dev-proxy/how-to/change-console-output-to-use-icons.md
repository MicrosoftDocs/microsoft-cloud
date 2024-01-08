---
title: Change console output to use icons
description: How to change console output to use icons
author: garrytrinder
ms.author: garrytrinder
ms.date: 1/08/2024
---

# Change console output to use icons

By default, the proxy console output uses [text labels](../technical-reference/Console-output-text-labels.md).

To change the label style, update the `labelMode` property value in the [devproxyrc.json](../technical-reference/devproxyrc.md) file stored in your installation directory.

## ASCII icons

To use ASCII icons, use:

```json
"labelMode": "icon",
```

## Nerdfont icons

To use NerdFont icons, first ensure that you have a compatible variable-width [font installed](https://www.nerdfonts.com/font-downloads), then use:

```json
"labelMode": "nerdFont"
```
