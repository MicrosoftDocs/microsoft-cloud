---
title: Proxy settings
description: Get started with Microsoft 365 Developer Proxy
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

Microsoft 365 Developer Proxy comes with several settings that you can use to control how the proxy should run.

You can configure these settings by setting them in the [m365proxyrc.json](./m365proxyrc.md) file, located in the proxy installation folder, or by setting them at run time.

The following table describes the settings.

Setting|Description|Command-line option|Allowed values|Default value
--|--|--|--|--
`port`|The port for the proxy server to listen on|`-p, --port <port>`|integer|`8000`
`log-level` | Level of messages to log|`--log-level <loglevel>`|string| `debug`, `info`, `warn`, `error`| `info`
`failureRate`|Rate of requests to Microsoft Graph between `0` and `100` that the proxy should fail. Set to `0` to pass all requests to Microsoft Graph, and to `100` to fail all requests.|`-f, --failure-rate <failurerate>`|`0..100`|`50`
`mocksFile`|Provide a file populated with mock responses|`--mocks-file <mocksfile>`| text |`responses.json`
`noMocks`|Don't use mock responses|`-n, --no-mocks`|`true`, `false`|`false`
`allowedErrors`|List of errors that the developer proxy might produce|`-a, --allowed-errors <allowederrors> `| See [Supported HTTP error status codes](./Supported-HTTP-error-status-codes.md)|All supported error codes
`urlsToWatch`|List of URLs allowed for testing|n\a|Absolute URL (can contain wildcards) for example, `"https://graph.microsoft.com/v1.0/*"`|See [m365proxyrc](./m365proxyrc.md) file
`labelMode`| Set the console output label mode |n\a|string| `text`, `icon`, `nerdFont`| `text`
