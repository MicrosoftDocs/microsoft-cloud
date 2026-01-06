---
title: Uninstall
description: How to uninstall Dev Proxy
author: garrytrinder
ms.author: garrytrinder
ms.date: 01/03/2026
zone_pivot_groups: client-operating-system
---

<!-- INTENT: Remove Dev Proxy from system -->
<!-- SOLUTION: Use package manager uninstall or manual removal -->
<!-- RESULT: Dev Proxy removed from system -->
<!-- PLUGINS: none -->
<!-- JOB: configure-proxy -->
<!-- TIME: 5 minutes -->

# Uninstall

> **At a glance**  
> **Goal:** Remove Dev Proxy from system  
> **Time:** 5 minutes  
> **Plugins:** None  
> **Prerequisites:** Dev Proxy installed

If you want to remove Dev Proxy from your machine, follow these steps.

::: zone pivot="client-operating-system-windows"

Uninstall Dev Proxy by running in the command prompt:

```console
winget uninstall DevProxy.DevProxy
```

::: zone-end

::: zone pivot="client-operating-system-macos"

Uninstall Dev Proxy by running in the command prompt:

```console
brew uninstall dev-proxy
```

::: zone-end

::: zone pivot="client-operating-system-linux"

1. Uninstall Dev Proxy by deleting the Dev Proxy installation folder
1. Remove Dev Proxy from your system path
    1. Open your shell profile in a text editor > `~/.bashrc`.
    1. Update `PATH` environment variable.
    1. Reload your profile > `source ~/.bashrc`.
1. Remove the Dev Proxy certificate from your machine.
    1. Remove the certificate > `sudo rm /usr/local/share/ca-certificates/dev-proxy-ca.crt`
    1. Update certificates > `sudo update-ca-certificates`

::: zone-end

## Dev Proxy Certificate Uninstallation

If you haven't removed the Dev Proxy certificate automatically during uninstallation, follow these steps to remove it manually:

::: zone pivot="client-operating-system-windows"

1. Open **Start Menu**.
1. In the search box, type `Manage user certificates`.
1. Select **Manage user certificates** that appears in the search list. This opens the **Certificates - Current User** window.
1. Remove the certificate from the **Personal** store:
    1. In the navigation pane on the left, expand the **Personal** folder.
    1. Select the **Certificates** subfolder.
    1. In the central pane, locate and select the **Dev Proxy CA** certificate.
    1. Press the <kbd>Delete</kbd> key on your keyboard, or right-click the certificate and choose **Delete** from the context menu.
    1. When prompted to confirm the deletion, select **Yes**.
1. Remove the certificate from the **Trusted Root Certification Authorities** store:
    1. In the navigation pane on the left, expand the **Trusted Root Certification Authorities** folder.
    1. Select the **Certificates** subfolder.
    1. In the central pane, locate and select the **Dev Proxy CA** certificate.
    1. Press the <kbd>Delete</kbd> key on your keyboard, or right-click the certificate and choose **Delete** from the context menu.
    1. When prompted to confirm the deletion, select **Yes**.

::: zone-end

::: zone pivot="client-operating-system-macos"

1. Open **Spotlight** by pressing <kbd>Command</kbd> + <kbd>Space</kbd>.
1. Open **Keychain Access** by typing `Keychain Access` and pressing <kbd>Return</kbd>.
1. Switch to the **My Certificates** category.
1. In the list of certificates, find the **Dev Proxy CA** certificate.
1. Right-click on the certificate, from the context menu that appears, select **Delete "Dev Proxy CA"**, and confirm the deletion.
1. Open **Finder**, locate the **~/Library/Application Support/dev-proxy** folder.
1. Remove the **dev-proxy** folder by opening the context menu and selecting **Move to Trash**.

::: zone-end

::: zone pivot="client-operating-system-linux"

1. Remove the certificate

    ```console
    sudo rm /usr/local/share/ca-certificates/dev-proxy-ca.crt
    ```

1. Update certificates

    ```console
    sudo update-ca-certificates
    ```

::: zone-end

## See also

- [Set up Dev Proxy](../get-started/set-up.md)
- [Configure Dev Proxy](../get-started/configure.md)
