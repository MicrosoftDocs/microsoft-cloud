---
title: The type initializer for 'Microsoft.Data.Sqlite.SqliteConnection' threw an exception
description: How to fix the type initializer for 'Microsoft.Data.Sqlite.SqliteConnection' threw an exception error
author: garrytrinder
ms.author: garrytrinder
ms.date: 01/03/2026
---

<!-- INTENT: Fix SQLite init error on macOS -->
<!-- SOLUTION: Install native SQLite library via Homebrew -->
<!-- RESULT: Dev Proxy starts without SQLite errors -->
<!-- PLUGINS: none -->
<!-- JOB: troubleshoot -->
<!-- TIME: 2 minutes -->

# The type initializer for 'Microsoft.Data.Sqlite.SqliteConnection' threw an exception

> **At a glance**  
> **Goal:** Fix SQLite init error on macOS  
> **Time:** 2 minutes  
> **Plugins:** None  
> **Prerequisites:** [Set up Dev Proxy](../get-started/set-up.md)

After starting the proxy, you see the following error:

```text
The type initializer for 'Microsoft.Data.Sqlite.SqliteConnection' threw an exception.
```

This error happens when macOS doesn't consider the `libe_sqlite3.dylib` file an executable.

Open the `dev-proxy` installation folder in a command prompt and execute `chmod +x libe_sqlite3.dylib`.

## See also

- [SQLite Error 11: database disk image is malformed](./sqlite-error-11-database-disk-image-is-malformed.md)
- [Get help and support](./get-help-and-support.md)
