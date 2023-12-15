---
title: The type initializer for 'Microsoft.Data.Sqlite.SqliteConnection' threw an exception
description: How to fix the type initializer for 'Microsoft.Data.Sqlite.SqliteConnection' threw an exception error
author: garrytrinder
ms.author: garrytrinder
ms.date: 11/03/2023
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

# The type initializer for 'Microsoft.Data.Sqlite.SqliteConnection' threw an exception

After starting the proxy, you see the following error:

```text
The type initializer for 'Microsoft.Data.Sqlite.SqliteConnection' threw an exception.
```

This error happens when macOS doesn't consider the `libe_sqlite3.dylib` file an executable.

Open the `dev-proxy` installation folder in a terminal and execute `chmod -x libe_sqlite3.dylib`.
