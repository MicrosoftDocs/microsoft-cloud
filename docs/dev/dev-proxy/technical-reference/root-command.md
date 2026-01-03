---
title: (root)
description: Dev Proxy root command reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/03/2026
---

<!-- INTENT: Reference for devproxy command line options -->
<!-- COMMAND: devproxy -->

# (root)

Starts Dev Proxy.

## Synopsis

```text
devproxy [options]

Options:
  -c, --config-file <file>      Configuration file path (default: devproxyrc.json)
  -u, --urls-to-watch <urls>    URLs to intercept (supports wildcards)
  -p, --port <port>             Proxy port (default: 8000)
  --log-level <level>           Logging level: trace|debug|information|warning|error
  --record                      Start recording immediately
  --discover                    Run in URL discovery mode
  --watch-pids <pids>           Process IDs to watch
  --watch-process-names <names> Process names to watch
  -t, --timeout <seconds>       Auto-stop after inactivity
  --version                     Show version
  -h, --help                    Show help
```

## Usage

```console
devproxy
```

## Arguments

None

## Options

|Name|Description|Allowed values|Default|
|--|--|--|--|
|`--as-system-proxy`|Whether to register Dev Proxy as the system proxy on startup. When set to `true` requires `installCert` to be, set to `true`|`true`, `false`|`true`|
|`-c, --config-file <configFile>`|The path to the configuration file|Local file path|`devproxyrc.json`|
|`--discover`|Run Dev Proxy in [discovery](../how-to/discover-urls-watch.md) mode|n/a|n/a|
|`-e, --env <env>`|Variables to set for the Dev Proxy process|string `key1=value`|n/a|
|`--install-cert`|Whether to install the root certificate|`true`, `false`|`true`|
|`--ip-address <ipAddress>`|The IP address for the proxy to bind to|IPv4 address|`127.0.0.1`|
|`--log-level <loglevel>`|Level of messages to log|`trace`, `debug`, `information`, `warning`, `error`|`information`|
|`--no-first-run`|Skip the first run experience (don't trust certificate on macOS)|n/a|n/a|
|`-p, --port <port>`|The port for the proxy server to listen on|integer|`8000`|
|`--record`|Use this option to record all request logs|n/a|n/a|
|`-t, --timeout <seconds>`|Automatically stop proxy after a period of inactivity|integer|n/a|
|`-u, --urls-to-watch <urlsToWatch>`|List of URLs that proxy should intercept|Absolute URL (can contain wildcards) for example, `"https://api.contoso.com/*"`|See [devproxyrc](./devproxyrc.md) file|
|`--version`|Show version information|n/a|n/a|
|`--watch-pids <pids>`|The IDs of processes to watch for requests|integer|n/a|
|`--watch-process-names <processNames>`|The names of processes to watch for requests|string|n/a|
|`-?, -h, --help`|Show help and usage information|n/a|n/a|
