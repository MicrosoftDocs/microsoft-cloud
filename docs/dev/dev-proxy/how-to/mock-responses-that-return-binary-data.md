---
title: Mock responses that return binary data
description: How to mock responses that return binary data
author: garrytrinder
ms.author: garrytrinder
ms.date: 11/03/2023
---

# Mock responses that return binary data

For some requests, you might want to respond with binary data like documents or images. 

In Dev Proxy, you can define a binary response by setting the `responseBody` to a string value that starts with `@` followed by file path relative to the current working directory, for example:

```json
{
  "responses": [
    {
      "url": "https://graph.microsoft.com/v1.0/users/*/photo/$value",
      "method":  "GET",
      "responseBody": "@picture.jpg",
      "responseHeaders": {
        "content-type": "image/jpeg"
      }
    }
  ]
}
```

When you call `GET https://graph.microsoft.com/v1.0/users/ben@contoso.com/photo/$value`, you get the image stored in the `picture.jpg` file in the current directory.

> [!CAUTION]
> If you are using the command line to execute the HTTP request, ensure that you have correctly escaped the `dollar` sign. See [Why is proxy not mocking my binary response](./Why-is-proxy-not-mocking-my-binary-response.md).
