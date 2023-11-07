In v0.12, we introduced support for mocking n-th request and extended the [response](./Response-object) object with a new property called `nth`.

Using the following mock file as an example, we can see that it contains two responses to the same request URL. Proxy uses the first response that uses the `nth` property, when it intercepts a request with the specified URL for the second time. For all other requests, the proxy returns the second response.

> ℹ️ Responses with the `nth` property should be first. Proxy uses responses based on the first match.

```json
{
  "responses": [
    {
      "url": "https://graph.microsoft.com/v1.0/external/connections/*/operations/*",
      "method": "GET",
      "nth": 2,
      "responseCode": 200,
      "responseBody": {
        "id": "1.neu.0278337E599FC8DBF5607ED12CF463E4.6410CCF8F6DB8758539FB58EB56BF8DC",
        "status": "completed",
        "error": null
      }
    },
    {
      "url": "https://graph.microsoft.com/v1.0/external/connections/*/operations/*",
      "method": "GET",
      "responseCode": 200,
      "responseBody": {
        "id": "1.neu.0278337E599FC8DBF5607ED12CF463E4.6410CCF8F6DB8758539FB58EB56BF8DC",
        "status": "inprogress",
        "error": null
      }
    }
  ]
}
```
