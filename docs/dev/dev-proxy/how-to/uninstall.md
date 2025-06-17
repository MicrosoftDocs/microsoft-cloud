---
title: Uninstall
description: How to uninstall Dev Proxy
author: garrytrinder
ms.author: garrytrinder
ms.date: 06/16/2025
zone_pivot_groups: client-operating-system
---

# Uninstall

If you want to remove Dev Proxy from your machine, follow these steps.

::: zone pivot="client-operating-system-windows"

Uninstall Dev Proxy by running in the command prompt:

```console
winget uninstall Microsoft.DevProxy
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

# Dev Proxy Certificate Uninstallation

In case Dev Proxy certificate removal is not performed automatically during uninstallation, follow these steps to remove it manually:

::: zone pivot="client-operating-system-windows"

1. Open **Start Menu**.
1. In the search box, type `Manage user certificates`.
1. Select **Manage user certificates** that appears in the search list. This will open the **Certificates - Current User** window.
1. Remove from the **Personal** store:
    1. In the navigation pane on the left, expand the **Personal** folder.
    1. Select the **Certificates** subfolder.
    1. In the central pane, locate and select the **Dev Proxy CA** certificate.
    1. Press the <kbd>Delete</kbd> key on your keyboard, or right-click the certificate and choose **Delete** from the context menu.
    1. Click **Yes** when prompted to confirm the deletion.
1. Remove from the **Trusted Root Certification Authorities** store:
    1. In the navigation pane on the left, expand the **Trusted Root Certification Authorities** folder.
    1. Select the **Certificates** subfolder.
    1. In the central pane, locate and select the **Dev Proxy CA** certificate.
    1. Press the <kbd>Delete</kbd> key on your keyboard, or right-click the certificate and choose **Delete** from the context menu.
    1. Click **Yes** when prompted to confirm the deletion.

::: zone-end

::: zone pivot="client-operating-system-macos"

1. Press <kbd>Command</kbd> + <kbd>Space</kbd> to open **Spotlight**.
1. Type `Keychain Access` and press <kbd>Return</kbd> to open **Keychain Access**.
1. Switch to **My Certificates** category.
1. In the list of certificates, find the **Dev Proxy CA** certificate.
1. Right-click on the certificate, from the context menu that appears, select **Delete "Dev Proxy CA"**, and confirm the deletion.
1. Open **Finder**, locate the **~/Library/Application Support/dev-proxy** folder.
1. Right-click on the **dev-proxy** folder, from the context menu that appears, select **Move to Trash** to remove it.

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
