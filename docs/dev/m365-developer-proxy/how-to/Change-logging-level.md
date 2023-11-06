By default, the proxy is set to log information level events. These events are non-request related messages, only the events related to the working of the Developer Proxy.

To change the default logging level, update the `logLevel` setting in [m365proxyrc.json](./m365proxyrc).

```json
"logLevel": "debug",
```

To change the logging level at run time, use the `--log-level` command line option.

```json
m365proxy --log-level debug
```
