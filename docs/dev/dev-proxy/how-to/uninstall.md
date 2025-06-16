---
title: Uninstall
description: How to uninstall Dev Proxy
author: garrytrinder
ms.author: garrytrinder
ms.date: 06/18/2024
zone_pivot_groups: client-operating-system
---

# Uninstall

If you want to remove Dev Proxy from your machine, follow these steps.

::: zone pivot="client-operating-system-windows"

1. Uninstall Dev Proxy by running in the command prompt: `winget uninstall Microsoft.DevProxy`
    > [!NOTE]
    > The uninstaller removes Dev Proxy application and its certificate.
1. Optional. Manually remove the Dev Proxy certificate from your machine.
    1. Console. Execute in console [`devproxy cert remove`](../technical-reference/cert-remove.md)
    1. User Certificates Manager
        1. Open `Start Menu`.
        1. In the search box, enter `Manage user certificates`.
        1. To open the `Certificates` dialog box, select the result in the list.
        1. `Personal` folder:
            1. In the tree view, expand the `Personal` folder and select the `Certificates` child folder.
            1. Remove the `Dev Proxy CA` certificate by selecting and pressing the <kbd>Delete</kbd> key on your keyboard, or right select and in the menu select `Delete`.
            1. To confirm the deletion, Select `Yes`.
        1. `Trusted Root Certification Authorities` folder:
            1. In the tree view and expand the `Trusted Root Certification Authorities` folder and select the `Certificates` child folder.
            1. Remove the `Dev Proxy CA` certificate by selecting and pressing the <kbd>Delete</kbd> key on your keyboard, or right select and in the menu select `Delete`.
            1. To confirm the deletion, Select `Yes`.

::: zone-end

::: zone pivot="client-operating-system-macos"

1. Uninstall Dev Proxy by running in the command prompt: `brew uninstall dev-proxy`
    > [!NOTE]
    > The uninstaller removes Dev Proxy application and its certificate.
1. Optional. Manually remove the Dev Proxy certificate from your machine.
    1. Console. Execute in console [`devproxy cert remove`](../technical-reference/cert-remove.md)
    1. Keychain Access
        1. Open `Keychain Access`.
        1. Switch to `My Certificates`.
        1. In the list of certificates, find the `Dev Proxy CA` certificate.
        1. Right select the certificate and from the menu, select `Delete Dev Proxy CA`.
        1. In Finder, locate and remove the `~/Library/Application Support/dev-proxy` folder.

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
