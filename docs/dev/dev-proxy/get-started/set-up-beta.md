---
title: Set up Dev Proxy beta
description: Learn how to install and run the beta version of Dev Proxy to try the latest preview features.
author: waldekmastykarz
ms.author: wmastyka
ms.date: 02/26/2026
ms.topic: get-started
zone_pivot_groups: client-operating-system
---

# Set up Dev Proxy beta

Dev Proxy beta gives you early access to the latest preview features before they're released in the stable version. If you want to try new capabilities and provide feedback, install the beta version.

> [!NOTE]
> Beta versions might contain bugs or incomplete features. For production testing scenarios, we recommend using the [stable version](set-up.md).

## Install Dev Proxy beta

::: zone pivot="client-operating-system-windows"

# [Automated](#tab/automated)

To install Dev Proxy beta using winget, run the following command:

```console
winget install DevProxy.DevProxy.Beta --silent
```

> [!IMPORTANT]
> Dev Proxy installer adds a new entry to PATH. To use Dev Proxy after installation, you must restart the command prompt to refresh the PATH environment variable.

# [Manual](#tab/manual)

[Download](https://aka.ms/devproxy/beta/) the latest beta and extract the files into a folder. For this tutorial, we assume you extract the files into a folder named `devproxy-beta` located in your home directory.

To start Dev Proxy beta from any directory, add its installation folder location to your PATH.

1. Open the `Start` menu.
1. Enter `Edit environment variables for your account` into the search box. Select the result in the list to open the `Environment Variables` dialog box.
1. In the `User variables for <username>` section, select the row with the variable name of `Path` and select the `Edit...` button.
1. In the `Edit environment variable` dialog box, select the `New` button.
1. Enter `%USERPROFILE%\devproxy-beta` into the new row and select `OK`.
1. To confirm changes, select `OK`.

---

::: zone-end

::: zone pivot="client-operating-system-macos"

# [Automated](#tab/automated)

To install Dev Proxy beta using Homebrew, run the following commands:

```console
brew tap dotnet/dev-proxy
brew install dev-proxy-beta
```

# [Manual](#tab/manual)

[Download](https://aka.ms/devproxy/beta/) the latest beta and extract the files into a folder. For this tutorial, we assume you extract the files into a folder named `devproxy-beta` located in your home directory.

To start Dev Proxy beta from any directory, add its installation folder location to your PATH.

The below steps show how to add the proxy to PATH when using [zsh](https://www.zsh.org/) shell. Depending on the shell you use, your profile file might differ.

1. Open your shell profile in a text editor > `~/.zshrc`.
1. Update `PATH` environment variable with location of the proxy > `export PATH=".:$PATH:$HOME/devproxy-beta"`.
1. Reload your profile > `source ~/.zshrc`.

---

::: zone-end

::: zone pivot="client-operating-system-linux"

# [Automated](#tab/automated)

To install Dev Proxy beta using the setup script, run the following commands:

```console
bash -c "$(curl -sL https://aka.ms/devproxy/setup-beta.sh)"
```

If you use PowerShell, run the following command:

```powershell
(Invoke-WebRequest https://aka.ms/devproxy/setup-beta.ps1).Content | Invoke-Expression
```

# [Manual](#tab/manual)

[Download](https://aka.ms/devproxy/beta/) the latest beta and extract the files into a folder. For this tutorial, we assume you extract the files into a folder named `devproxy-beta` located in your home directory.

To start Dev Proxy beta from any directory, add its installation folder location to your PATH.

The below steps show how to add the proxy to PATH when using bash shell. Depending on the shell you use, your profile file might differ.

1. Open your shell profile in a text editor > `~/.bashrc`.
1. Update `PATH` environment variable with location of the proxy > `export PATH=".:$PATH:$HOME/devproxy-beta"`.
1. Reload your profile > `source ~/.bashrc`.

---

::: zone-end

## Run Dev Proxy beta

To run the beta version of Dev Proxy, use the `devproxy-beta` command:

```console
devproxy-beta
```

The first time you run Dev Proxy beta, follow the same certificate trust and firewall steps as described in the [stable version setup](set-up.md#start-dev-proxy-for-the-first-time).

## Next step

> [!div class="nextstepaction"]
> [Configure Dev Proxy](./configure.md)
