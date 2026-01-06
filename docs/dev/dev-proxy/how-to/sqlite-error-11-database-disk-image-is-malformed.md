---
title: "SQLite Error 11: 'database disk image is malformed'"
description: "How to fix the `SQLite Error 11: 'database disk image is malformed'` error"
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/03/2026
---

<!-- INTENT: Fix corrupted SQLite database -->
<!-- SOLUTION: Delete and refresh the local database file -->
<!-- RESULT: Dev Proxy database restored to working state -->
<!-- PLUGINS: none -->
<!-- JOB: troubleshoot -->
<!-- TIME: 5 minutes -->

# SQLite Error 11: 'database disk image is malformed'

> **At a glance**  
> **Goal:** Fix corrupted SQLite database  
> **Time:** 5 minutes  
> **Plugins:** None  
> **Prerequisites:** [Set up Dev Proxy](../get-started/set-up.md)

After starting Dev Proxy, you see the following error:

```text
SQLite Error 11: 'database disk image is malformed'.
```

This error happens when Dev Proxy failed updating the SQLite database previously.

Dev Proxy uses a SQLite database for the [GraphSelectGuidancePlugin](../technical-reference/graphselectguidanceplugin.md) to know which Microsoft Graph endpoints support the `$select` parameter. Dev Proxy automatically updates the database when it starts using Microsoft Graph API metadata. Updating the database can fail, for example,  because you close Dev Proxy when it's writing information to the database. It corrupts the database and you start seeing the aforementioned error.

To fix the error, rebuild the database using the [`msgraphdb`](../technical-reference/msgraphdb.md) command by running:

```console
devproxy msgraphdb
```

## See also

- [Refresh local Microsoft Graph database](./refresh-local-microsoft-graph-database.md)
- [msgraphdb command](../technical-reference/msgraphdb.md)
- [GraphSelectGuidancePlugin](../technical-reference/graphselectguidanceplugin.md)
