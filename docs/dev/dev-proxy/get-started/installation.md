---
title: Installation
description: Install Dev Proxy
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

# Installation

If you do run into any difficulties, don’t hesitate to contact us by raising a [new issue](https://github.com/microsoft/dev-proxy/issues/new) and we're glad to help you out.

## Download latest release

1. [Download](https://github.com/microsoft/dev-proxy/releases/latest) the ZIP file for your operating system.
2. Extract the contents of the ZIP into a folder.

> [!NOTE]
> The tutorial steps assume that you have saved the files in your home directory in a folder named `dev-proxy`, but you can store them anywhere.

## Make the proxy globally available

Making the proxy globally available enables starting the proxy from any directory and project specific mock responses.

Updating the path differs across operating systems. Follow the next steps relevant to the operating system that you use.

### [Windows](#tab/windows)

  1. Open the `Start` menu.
  1. Enter `Edit environment variables for your account` into the search box, select the result in the list to open the `Environment Variables` dialog box.
  1. In the `User variables for <username>` section, select the row with the variable name of `Path` and select the `Edit...` button.
  1. In the `Edit environment variable` dialog box, select the `New` button.
  1. Enter `%USERPROFILE%\dev-proxy` into the new row and select `OK`.
  1. Select `OK` to confirm changes.

### [macOS](#tab/macos)

The below steps show how to add the proxy to PATH when using [zsh](https://www.zsh.org/) shell. Depending on the shell you use, your profile file might differ.

  1. Open your shell profile in a text editor > `~/.zshrc`.
  1. Update `PATH` environment variable with location of the proxy > `export PATH=".:$PATH:$HOME/dev-proxy"`.
  1. Reload your profile > `source ~/.zshrc`.

---

> [!div class="nextstepaction"]
> [Use proxy for the first time](./using-the-proxy-for-the-first-time.md)
