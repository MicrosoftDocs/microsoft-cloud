---
title: Proxy settings
description: Overview of proxy settings
author: garrytrinder
ms.author: garrytrinder
ms.date: 02/28/2024
---

# Proxy settings

Dev Proxy comes with several settings that you can use to control how the proxy should run.

You can configure these settings by setting them in the [devproxyrc.json](./devproxyrc.md) file, located in the proxy installation folder, or by setting them at run time through command line options.

The following table describes the settings.

|Setting|Description|Command-line option|Allowed values|Default value|
|--|--|--|--|--|
|`asSystemProxy`|Whether to register Dev Proxy as the system proxy on startup. When set to `true` requires `installCert` to be set to `true`|`--as-system-proxy`|`true`, `false`|`true`|
|`installCert`|Whether to install the root certificate|`--install-cert`|`true`, `false`|`true`|
|`ipAddress`|The IP address for the proxy to bind to|`--ip-address <ipAddress>`|IPv4 address|`127.0.0.1`|
|`labelMode`| Set the console output label mode |n/a|`text`, `icon`, `nerdFont`| `text`|
|`logLevel`|Level of messages to log|`--log-level <loglevel>`|`debug`, `information`, `warning`, `error`| `information`|
|`newVersionNotification`|Whether to notify about new versions|n/a|`none`, `stable`, `beta`|`stable`|
|n/a|Skip the first run experience (don't trust certificate on macOS)|`--no-first-run`|n/a|n/a|
|`port`|The port for the proxy server to listen on|`-p, --port <port>`|integer|`8000`|
|`rate`|The percentage of chance that proxy fails a request. Set to `0` to pass all requests to APIs, and to `100` to fail all requests.|`-f, --failure-rate <failurerate>`|`0..100`|`50`|
|`record`|Use this option to record all request logs|`--record`|n/a|n/a|
|`urlsToWatch`|List of URLs that proxy should intercept|`-u, --urls-to-watch <urlsToWatch>`|Absolute URL (can contain wildcards) for example, `"https://api.contoso.com/*"`|See [devproxyrc](./devproxyrc.md) file|
|n/a|The IDs of processes to watch for requests|`--watch-pids <pids>`|integer|n/a|
|n/a|The names of processes to watch for requests|`--watch-process-names <processNames>`|string|n/a|
