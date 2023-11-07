By default, the proxy is set to log information level events. These events are messages not related to requests, only the events related to the working of the Developer Proxy.

To change the default logging level, update the `logLevel` setting in the `m365proxyrc.json` file.

```json
"logLevel": "debug",
```

To change the logging level at run time, use the `--log-level` command line option.

```json
m365proxy --log-level debug
```
