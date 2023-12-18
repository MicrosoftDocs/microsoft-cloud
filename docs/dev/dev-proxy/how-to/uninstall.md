---
title: Uninstall
description: How to uninstall Dev Proxy
author: garrytrinder
ms.author: garrytrinder
ms.date: 12/18/2023
ms.topic: how-to
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
  - tool=devproxy
---

# Uninstall

If you want to remove Dev Proxy from your machine, follow the following steps.

1. Remove the folder where you extracted the files from the ZIP on your disk.
2. Remove the certificate, following the steps relevant to the OS you use.

<details>
   <summary>Windows 11</summary>

1. Open `Start Menu`
2. Enter `Manage user certificates` in the search box, select the result in the list to open the `Certificates` dialog box.
3. In the tree view, expand the `Personal` folder and select the `Certificates` child folder.
4. Remove the certificate issued to `Titanium Root Certificate Authority` by selecting and pressing the Delete key on your keyboard, or right select and select `Delete` in the menu.
5. Select `Yes` to confirm the deletion.
</details>

<details>
   <summary>macOS Monterey</summary>

1.  Remove the `~/Library/Application Support/dev-proxy` folder.
</details>

Dev Proxy doesn't create any extra files or registry entries on your machine.
