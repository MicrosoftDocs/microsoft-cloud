---
title: Proxy settings
description: Overview of proxy settings
author: garrytrinder
ms.author: garrytrinder
ms.date: 08/23/2024
---

# Proxy settings

Dev Proxy comes with several settings that you can use to control how the proxy should run.

You can configure these settings by setting them in the [devproxyrc.json](./devproxyrc.md) file, located in the proxy installation folder, or by setting them at run time through command line options.

The following table describes the settings.

|Setting|Description|Command-line option|Allowed values|Default value|
|--|--|--|--|--|
|`apiPort`|The port for the Dev Proxy API to listen on|n/a|integer|`8897`|
|`asSystemProxy`|Whether to register Dev Proxy as the system proxy on startup. When set to `true` requires `installCert` to be, set to `true`|`--as-system-proxy`|`true`, `false`|`true`|
|`filterByHeaders`|Only intercept requests with specific headers|n/a|`{"filterByHeaders": [ { "name": "value" } ] }`. Value can be empty to include requests with the specified header no matter its value.|n/a|
|`installCert`|Whether to install the root certificate|`--install-cert`|`true`, `false`|`true`|
|`ipAddress`|The IP address for the proxy to bind to|`--ip-address <ipAddress>`|IPv4 address|`127.0.0.1`|
|`languageModel`|Settings for the language model|n/a|See the [language model](../how-to/use-language-model.md) section for more information.|n/a|
|`logLevel`|Level of messages to log|`--log-level <loglevel>`|`trace`, `debug`, `information`, `warning`, `error`| `information`|
|`newVersionNotification`|Whether to notify about new versions|n/a|`none`, `stable`, `beta`|`stable`|
|n/a|Skip the first run experience (don't trust certificate on macOS)|`--no-first-run`|n/a|n/a|
|`port`|The port for the proxy server to listen on|`-p, --port <port>`|integer|`8000`|
|`rate`|The percentage of chance that proxy fails a request. Set to `0` to pass all requests to APIs, and to `100` to fail all requests.|`-f, --failure-rate <failurerate>`|`0..100`|`50`|
|`record`|Use this option to record all request logs|`--record`|n/a|n/a|
|`urlsToWatch`|List of URLs that proxy should intercept|`-u, --urls-to-watch <urlsToWatch>`|Absolute URL (can contain wildcards) for example, `"https://api.contoso.com/*"`|See [devproxyrc](./devproxyrc.md) file|
|n/a|The IDs of processes to watch for requests|`--watch-pids <pids>`|integer|n/a|
|n/a|The names of processes to watch for requests|`--watch-process-names <processNames>`|string|n/a|
