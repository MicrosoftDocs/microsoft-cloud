---
title: Uninstall
description: How to uninstall Dev Proxy
author: garrytrinder
ms.author: garrytrinder
ms.date: 02/28/2024
zone_pivot_groups: client-operating-system
---

# Uninstall

If you want to remove Dev Proxy from your machine, follow these steps.

1. Remove the folder where you extracted the files from the ZIP on your disk.
1. Remove the certificate, following the steps relevant to the OS you use.

::: zone pivot="client-operating-system-windows"

1. Open `Start Menu`.
1. In the search box, Enter `Manage user certificates`.
1. To open the `Certificates` dialog box, select the result in the list.
1. In the tree view, expand the `Personal` folder and select the `Certificates` child folder.
1. Remove the `Dev Proxy CA` certificate by selecting and pressing the <kbd>Delete</kbd> key on your keyboard, or right select and in the menu select `Delete`.
1. To confirm the deletion, Select `Yes`.

::: zone-end

::: zone pivot="client-operating-system-macos"

1. Open `Keychain Access`.
1. Switch to `My Certificates`.
1. In the list of certificates, find the `Dev Proxy CA` certificate.
1. Right select the certificate and from the menu, select `Delete Dev Proxy CA`.
1. In Finder, locate and remove the `~/Library/Application Support/dev-proxy` folder.

::: zone-end

Dev Proxy doesn't create any extra files or registry entries on your machine.
