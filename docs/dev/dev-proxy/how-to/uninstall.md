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

1. Open `Start Menu`
1. Enter `Manage user certificates` in the search box, select the result in the list to open the `Certificates` dialog box.
1. In the tree view, expand the `Personal` folder and select the `Certificates` child folder.
1. Remove the certificate issued to `Titanium Root Certificate Authority` by selecting and pressing the Delete key on your keyboard, or right select and select `Delete` in the menu.
1. Select `Yes` to confirm the deletion.

::: zone-end

::: zone pivot="client-operating-system-macos"

1. Remove the `~/Library/Application Support/dev-proxy` folder.

::: zone-end

Dev Proxy doesn't create any extra files or registry entries on your machine.
