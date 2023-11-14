---
title: Refresh local Microsoft Graph database
description: How to refresh the local Microsoft Graph database
author: garrytrinder
ms.author: garrytrinder
ms.contributors: garrytrinder
ms.date: 11/03/2023
ms.topic: conceptual
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
---

# Refresh local Microsoft Graph database

We introduced a local SQLite database to store the OpenAPI specifications for both the v1.0 and beta endpoints of Microsoft Graph in v0.11 release. This change significantly improved the performance and accuracy of the [GraphSelectGuidancePlugin](../technical-reference/GraphSelectGuidancePlugin.md), which returns guidance when requests made to endpoints that support `$select` option but aren't being used.

Whilst we automatically create and update this local database for you, there maybe occasions when you want to refresh this database yourself.

Execute `devproxy msgraphdb` in your proxy installation folder to rebuild and update your local database.

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
