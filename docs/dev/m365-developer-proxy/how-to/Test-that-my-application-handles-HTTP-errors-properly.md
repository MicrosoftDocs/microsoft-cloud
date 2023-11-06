When applications use Microsoft Graph and other cloud services, it can happen, that these APIs are temporarily unavailable.

It's important, that applications consider such scenario and handle exceptions properly.

Testing exceptions in APIs you don't manage is hard, because it's hard to make the API return a specific response.

Using Microsoft 365 Developer Proxy, you can mimic erroneous responses from Microsoft Graph and test your application to see that it handles these errors properly.

To test your application, start the Microsoft 365 Developer Proxy:

```cmd
m365proxy --no-mocks
```
