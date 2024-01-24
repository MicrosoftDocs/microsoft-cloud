---
title: Refresh local Microsoft Graph database
description: How to refresh the local Microsoft Graph database
author: garrytrinder
ms.author: garrytrinder
ms.date: 01/24/2024
---

# Refresh local Microsoft Graph database

The [GraphSelectGuidancePlugin](../technical-reference/GraphSelectGuidancePlugin.md) uses a local SQLite database to store the OpenAPI specifications for both the v1.0 and beta endpoints of Microsoft Graph. This database is automatically created and updated for you, but there maybe occasions when you want to refresh this database yourself.

Execute [`devproxy msgraphdb`](../technical-reference/msgraphdb.md) in your proxy installation folder to rebuild and update your local database.

Here's sample output of running the command.

```text
Checking for updated OpenAPI files...
Downloaded OpenAPI file from https://raw.githubusercontent.com/microsoftgraph/msgraph-metadata/master/openapi/v1.0/openapi.yaml to <devproxy-path>\graph-v1_0-openapi.yaml
Downloaded OpenAPI file from https://raw.githubusercontent.com/microsoftgraph/msgraph-metadata/master/openapi/beta/openapi.yaml to <devproxy-path>\graph-beta-openapi.yaml
Loading OpenAPI files...
Creating database...
Filling database...
Inserted 17306 endpoints in the database
Microsoft Graph database successfully updated
```
