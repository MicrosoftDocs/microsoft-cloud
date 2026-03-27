---
title: Upgrade Dev Proxy
description: Learn how to upgrade Dev Proxy between stable and beta versions
author: waldekmastykarz
ms.author: wmastyka
ms.date: 03/26/2026
ms.topic: how-to
zone_pivot_groups: client-operating-system
---

<!-- INTENT: Upgrade Dev Proxy to a newer version -->
<!-- SOLUTION: Use package manager or manual upgrade -->
<!-- RESULT: Dev Proxy upgraded to desired version -->
<!-- PLUGINS: none -->
<!-- JOB: configure-proxy -->
<!-- TIME: 5 minutes -->

# Upgrade Dev Proxy

> **At a glance**  
> **Goal:** Upgrade Dev Proxy to a newer version  
> **Time:** 5 minutes  
> **Plugins:** None  
> **Prerequisites:** Dev Proxy installed

This article explains how to upgrade Dev Proxy between stable and beta versions.

## Check your current version

Before upgrading, check which version of Dev Proxy you currently have installed:

```console
devproxy --version
```

If you have the beta version installed, run:

```console
devproxy-beta --version
```

## Before you upgrade

Before upgrading Dev Proxy, consider the following:

- **Back up configuration files in the installation folder**. Upgrading overwrites all files in the installation folder, including the default `devproxyrc.json`, the preset configurations in the `config` folder, and any configs you downloaded with `devproxy config get`. If you modified any of these files, back them up before upgrading. To avoid this risk, store your configuration and mock files in your project folder and pass them using `--config-file`. Files stored outside the installation folder aren't affected by upgrades.
- **Review the release notes**. Check the [release notes](https://github.com/dotnet/dev-proxy/releases) for any breaking changes or new features that might affect your workflow.
- **Configuration compatibility**. Dev Proxy configuration files include a schema reference. After upgrading, update the schema version in your configuration files to match the new version. For example, update `"$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.3.0/rc.schema.json"` to reference the new version.

## Upgrade stable to stable

To upgrade from one stable version to another stable version of Dev Proxy:

::: zone pivot="client-operating-system-windows"

# [Automated](#tab/automated)

Run the following command in any command prompt:

```console
winget upgrade DevProxy.DevProxy --silent
```

> [!NOTE]
> The `winget upgrade` command works regardless of which folder you run it from. The installer updates Dev Proxy in its installed location.

# [Manual](#tab/manual)

1. [Download](https://aka.ms/devproxy/download/) the latest release.
1. Extract the files to your existing Dev Proxy installation folder, overwriting the existing files.

---

::: zone-end

::: zone pivot="client-operating-system-macos"

# [Automated](#tab/automated)

Run the following commands:

```console
brew update
brew upgrade dev-proxy
```

# [Manual](#tab/manual)

1. [Download](https://aka.ms/devproxy/download/) the latest release.
1. Extract the files to your existing Dev Proxy installation folder, overwriting the existing files.

---

::: zone-end

::: zone pivot="client-operating-system-linux"

# [Automated](#tab/automated)

Run the setup script again to install the latest version:

```console
bash -c "$(curl -sL https://aka.ms/devproxy/setup.sh)"
```

If you use PowerShell:

```powershell
(Invoke-WebRequest https://aka.ms/devproxy/setup.ps1).Content | Invoke-Expression
```

# [Manual](#tab/manual)

1. [Download](https://aka.ms/devproxy/download/) the latest release.
1. Extract the files to your existing Dev Proxy installation folder, overwriting the existing files.

---

::: zone-end

## Upgrade stable to beta

To try the latest preview features, you can install the beta version alongside your stable installation. The beta version uses a separate command (`devproxy-beta`) and doesn't affect your stable installation.

::: zone pivot="client-operating-system-windows"

# [Automated](#tab/automated)

Run the following command:

```console
winget install DevProxy.DevProxy.Beta --silent
```

# [Manual](#tab/manual)

1. [Download](https://aka.ms/devproxy/beta/) the latest beta release.
1. Extract the files into a separate folder from your stable installation.
1. Add the new folder location to your PATH.

---

::: zone-end

::: zone pivot="client-operating-system-macos"

# [Automated](#tab/automated)

Run the following commands:

```console
brew tap dotnet/dev-proxy
brew install dev-proxy-beta
```

# [Manual](#tab/manual)

1. [Download](https://aka.ms/devproxy/beta/) the latest beta release.
1. Extract the files into a separate folder from your stable installation.
1. Add the new folder location to your PATH.

---

::: zone-end

::: zone pivot="client-operating-system-linux"

# [Automated](#tab/automated)

Run the following command:

```console
bash -c "$(curl -sL https://aka.ms/devproxy/setup-beta.sh)"
```

If you use PowerShell:

```powershell
(Invoke-WebRequest https://aka.ms/devproxy/setup-beta.ps1).Content | Invoke-Expression
```

# [Manual](#tab/manual)

1. [Download](https://aka.ms/devproxy/beta/) the latest beta release.
1. Extract the files into a separate folder from your stable installation.
1. Add the new folder location to your PATH.

---

::: zone-end

To run the beta version, use `devproxy-beta` instead of `devproxy`.

## Upgrade beta to stable

If you're running the beta version and want to switch to the stable version:

::: zone pivot="client-operating-system-windows"

# [Automated](#tab/automated)

First, uninstall the beta version:

```console
winget uninstall DevProxy.DevProxy.Beta
```

Then, install or upgrade the stable version:

```console
winget install DevProxy.DevProxy --silent
```

# [Manual](#tab/manual)

1. Delete your beta installation folder.
1. Remove the beta folder from your PATH.
1. [Download](https://aka.ms/devproxy/download/) the latest stable release.
1. Extract the files and set up your PATH as described in [Set up Dev Proxy](../get-started/set-up.md).

---

::: zone-end

::: zone pivot="client-operating-system-macos"

# [Automated](#tab/automated)

First, uninstall the beta version:

```console
brew uninstall dev-proxy-beta
```

Then, install or upgrade the stable version:

```console
brew tap dotnet/dev-proxy
brew install dev-proxy
```

If you already have the stable version installed, upgrade it:

```console
brew upgrade dev-proxy
```

# [Manual](#tab/manual)

1. Delete your beta installation folder.
1. Remove the beta folder from your PATH.
1. [Download](https://aka.ms/devproxy/download/) the latest stable release.
1. Extract the files and set up your PATH as described in [Set up Dev Proxy](../get-started/set-up.md).

---

::: zone-end

::: zone pivot="client-operating-system-linux"

1. Delete your beta installation folder.
1. Remove the beta folder from your PATH.
1. Install the stable version using the setup script:

    ```console
    bash -c "$(curl -sL https://aka.ms/devproxy/setup.sh)"
    ```

    If you use PowerShell:

    ```powershell
    (Invoke-WebRequest https://aka.ms/devproxy/setup.ps1).Content | Invoke-Expression
    ```

::: zone-end

## Upgrade beta to beta

To upgrade from one beta version to another:

::: zone pivot="client-operating-system-windows"

# [Automated](#tab/automated)

```console
winget upgrade DevProxy.DevProxy.Beta --silent
```

# [Manual](#tab/manual)

1. [Download](https://aka.ms/devproxy/beta/) the latest beta release.
1. Extract the files to your existing beta installation folder, overwriting the existing files.

---

::: zone-end

::: zone pivot="client-operating-system-macos"

# [Automated](#tab/automated)

```console
brew update
brew upgrade dev-proxy-beta
```

# [Manual](#tab/manual)

1. [Download](https://aka.ms/devproxy/beta/) the latest beta release.
1. Extract the files to your existing beta installation folder, overwriting the existing files.

---

::: zone-end

::: zone pivot="client-operating-system-linux"

# [Automated](#tab/automated)

Run the setup script again:

```console
bash -c "$(curl -sL https://aka.ms/devproxy/setup-beta.sh)"
```

If you use PowerShell:

```powershell
(Invoke-WebRequest https://aka.ms/devproxy/setup-beta.ps1).Content | Invoke-Expression
```

# [Manual](#tab/manual)

1. [Download](https://aka.ms/devproxy/beta/) the latest beta release.
1. Extract the files to your existing beta installation folder, overwriting the existing files.

---

::: zone-end

## See also

- [Set up Dev Proxy](../get-started/set-up.md)
- [Configure Dev Proxy](../get-started/configure.md)
- [Uninstall Dev Proxy](uninstall.md)
