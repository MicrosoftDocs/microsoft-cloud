---
title: Proxy settings
description: Overview of proxy settings
author: garrytrinder
ms.author: garrytrinder
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

# Proxy settings

Dev Proxy comes with several settings that you can use to control how the proxy should run.

You can configure these settings by setting them in the [devproxyrc.json](./devproxyrc.md) file, located in the proxy installation folder, or by setting them at run time.

The following table describes the settings.

|Settings|Description|Command-line option|Allowed values|Default value|
--|--|--|--|--
`allowedErrors`|List of errors that Dev Proxy might produce|`-a, --allowed-errors <allowederrors>`| See [Supported HTTP error status codes](./Supported-HTTP-error-status-codes.md)|All supported error codes
`failureRate`|Rate of requests to Microsoft Graph between `0` and `100` that the proxy should fail. Set to `0` to pass all requests to Microsoft Graph, and to `100` to fail all requests.|`-f, --failure-rate <failurerate>`|`0..100`|`50`
`ipAddress`|The IP address for the proxy to bind to|`--ip-address <ipAddress>`|IPv4 address|`127.0.0.1`
`labelMode`| Set the console output label mode |n/a|`text`, `icon`, `nerdFont`| `text`
`logLevel`|Level of messages to log|`--log-level <loglevel>`|`debug`, `info`, `warn`, `error`| `info`
`mocksFile`|Provide a file populated with mock responses|`--mocks-file <mocksfile>`| text |`responses.json`
`noMocks`|Don't use mock responses|`-n, --no-mocks`|`true`, `false`|`false`
`port`|The port for the proxy server to listen on|`-p, --port <port>`|integer|`8000`
`urlsToWatch`|List of URLs allowed for testing|n/a|Absolute URL (can contain wildcards) for example, `"https://graph.microsoft.com/v1.0/*"`|See [devproxyrc](./devproxyrc.md) file
