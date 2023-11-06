Typically, testing throttling is hard because it occurs rarely, when Microsoft 365 servers are under heavy load. Using the Microsoft 365 Developer Proxy, you can mimic throttling responses, and check if your application handles it correctly.

To test your application, start the Microsoft 365 Developer Proxy, configuring it to respond specifically with throttling responses, intercepting a high percentage of requests:

```cmd
m365proxy --allowed-errors 429 --failure-rate 90 --no-mocks
```

Next, run your application, and verify that it doesn't break when it gets 429 responses but waits for the amount of time specified on the response.

When testing your app, pay attention to the Microsoft 365 Developer Proxy output.

If your application backs-off when throttled, but doesn't wait for the amount of time specified on the requests, you'll see a message similar to `Calling https://graph.microsoft.com/v1.0/endpoint again before waiting for the Retry-After period. Request will be throttled`.

This message indicates that your application is not handling throttling correctly and will keep being throttled.