By default, Microsoft 365 Developer Proxy randomly responds with [supported error codes](../technical-reference/Supported-HTTP-error-status-codes.md) with a 50% chance that the proxy will intercept a request to Microsoft Graph.

You can increase or decrease the likelihood to a higher value or lower value by changing the `failure-rate` setting value.

```cmd
m365proxy --failure-rate 80
```
