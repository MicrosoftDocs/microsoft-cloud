---
title: Use Dev Proxy with Node.js applications
description: How to use Dev Proxy with Node.js applications
author: waldekmastykarz
ms.author: wmastyka
ms.date: 02/05/2024
---

# Use Dev Proxy with Node.js applications

There's no standard way of enabling proxies for Node.js applications. Whether you can use proxy, depends on the library you're using to make HTTP requests. Typically, you need to update your code to configure the proxy. You can however use the `global-agent` package to enable proxy support for your Node.js application with minimal code changes.

## Native Node.js fetch API

In v17.5.0, Node.js introduces experimental support for the `fetch` API. Unfortunately, this API is still limited and doesn't support configuring a proxy. If you want to use Dev Proxy with Node.js, you need to use a different library for making HTTP requests.

## `global-agent`

[`global-agent`](https://www.npmjs.com/package/global-agent) is a popular library that provides a global HTTP/HTTPS agent for Node.js. It allows you to specify the proxy using environment variables. The benefit of using `global-agent` is that you don't need to change how you issue HTTP requests in your application to use Dev Proxy.

Here's an example of how you can use `global-agent` in a Node.js application that uses `node-fetch`

```javascript
import fetch from 'node-fetch';
import { bootstrap } from 'global-agent';
bootstrap();

(async () => {
  const result = await fetch('https://jsonplaceholder.typicode.com/posts');
  const jsonResult = await result.json();
  console.log(JSON.stringify(jsonResult, null, 2));
})();
```

When starting your application, specify the proxy using the `GLOBAL_AGENT_HTTP_PROXY` environment variable and ignore certificate errors.

```bash
NODE_TLS_REJECT_UNAUTHORIZED=0 GLOBAL_AGENT_HTTP_PROXY=http://127.0.0.1:8000 node global-node-fetch.mjs
```

## node-fetch

[node-fetch](https://www.npmjs.com/package/node-fetch) is a popular library that provides a `fetch` implementation for Node.js. `node-fetch` doesn't support specifying the proxy using environment variables. Instead, you need to create a custom agent and pass it to the `fetch` method.

Here's an example of how you can use `node-fetch` with Dev Proxy by defining an agent using the `https-proxy-agent` package.

```javascript
const fetch = require('node-fetch');
const { HttpsProxyAgent } = require('https-proxy-agent');

(async () => {
  // Create a custom agent pointing to Dev Proxy
  const agent = new HttpsProxyAgent('http://127.0.0.1:8000');
  // Pass the agent to the fetch method
  const result = await fetch('https://jsonplaceholder.typicode.com/posts', { agent });
  const jsonResult = await result.json();
  console.log(JSON.stringify(jsonResult, null, 2));
})();
```

### Axios

[Axios](https://www.npmjs.com/package/axios) is another popular library for making HTTP requests in Node.js. Axios allows you to specify the proxy using environment variables or specify the agent directly in the request configuration.

### Use Axios and Dev Proxy with environment variables

When you use Dev Proxy with Axios and specify the proxy using environment variables, you don't need to change your code. All you need to do is to set the `https_proxy` environment variable and Axios uses it to make requests.

```javascript
import axios from 'axios';

(async () => {
  const result = await axios.get('https://jsonplaceholder.typicode.com/posts');
  const response = result.data;
  console.log(JSON.stringify(response, null, 2));
})();
```

Specify the `https_proxy` environment variable either globally or when starting your app.

```bash
https_proxy=http://127.0.0.1:8000 node axios.mjs
```

### Got

Similar to `node-fetch`, [Got](https://www.npmjs.com/package/got) doesn't support specifying the proxy using environment variables. Instead, you need to create a custom agent and pass it to the request.

Here's an example of how you can use Got with Dev Proxy:

```javascript
import got from 'got';
import { HttpsProxyAgent } from 'https-proxy-agent';

(async () => {
  // Create a custom agent pointing to Dev Proxy
  const agent = new HttpsProxyAgent('http://127.0.0.1:8000');
  const result = await got('https://jsonplaceholder.typicode.com/posts', {
    // Pass the agent to the fetch method
    agent: {
      https: agent
    },
    // Disable certificate validation
    https: {
      rejectUnauthorized: false
    }
  }).json();
  console.log(JSON.stringify(result, null, 2));
})();
```

### SuperAgent

[`SuperAgent`](https://www.npmjs.com/package/superagent) doesn't support specifying the proxy using environment variables. To use Dev Proxy with SuperAgent, you need to install the `superagent-proxy` plugin and configure the proxy using the `proxy` method.

```javascript
const superagent = require('superagent');
require('superagent-proxy')(superagent);

(async () => {
  const result = await superagent
    .get('https://jsonplaceholder.typicode.com/posts')
    .proxy('http://127.0.0.1:8000')
    // Disable certificate validation
    .disableTLSCerts();
  console.log(JSON.stringify(result.body, null, 2));
})();
```

## Known issues

When you use Dev Proxy with Node.js, you might encounter the following issues.

### `UNABLE_TO_VERIFY_LEAF_SIGNATURE` error

When you use Dev Proxy with Node.js, you get an error similar to:

```plaintext
/Users/user/my-app/node_modules/node-fetch/lib/index.js:1501
                        reject(new FetchError(`request to ${request.url} failed, reason: ${err.message}`, 'system', err));
                               ^
FetchError: request to https://jsonplaceholder.typicode.com/posts failed, reason: unable to verify the first certificate
    at ClientRequest.<anonymous> (/Users/user/my-app/node_modules/node-fetch/lib/index.js:1501:11)
    at ClientRequest.emit (node:events:518:28)
    at TLSSocket.socketErrorListener (node:_http_client:495:9)
    at TLSSocket.emit (node:events:518:28)
    at emitErrorNT (node:internal/streams/destroy:169:8)
    at emitErrorCloseNT (node:internal/streams/destroy:128:3)
    at process.processTicksAndRejections (node:internal/process/task_queues:82:21) {
  type: 'system',
  errno: 'UNABLE_TO_VERIFY_LEAF_SIGNATURE',
  code: 'UNABLE_TO_VERIFY_LEAF_SIGNATURE'
}
```

To fix this issue, you need to set the `NODE_TLS_REJECT_UNAUTHORIZED` environment variable to `0`. You can define it globally, or in-line when starting your app.

```bash
NODE_TLS_REJECT_UNAUTHORIZED=0 node index.js
```
