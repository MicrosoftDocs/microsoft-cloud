---
title: Use Dev Proxy with Node.js applications running in Docker
description: How to use Dev Proxy with Node.js applications running in Docker containers
author: waldekmastykarz
ms.author: wmastyka
ms.date: 04/08/2024
---

# Use Dev Proxy with Node.js applications running in Docker

If you run your Node.js application in a Docker container and want to use Dev Proxy, follow the general guidance for [using Dev Proxy with Node.js applications](./use-dev-proxy-with-nodejs.md).

Because your app is running inside a Docker container and Dev Proxy is running on your host, you need to configure the proxy to point to the IP address of your computer.

When using Dev Proxy on macOS, you need to attach it to the `0.0.0.0` address to make it accessible from the Docker container. To configure the IP address for Dev Proxy, start it with the following command:

```console
devproxy --ip-address 0.0.0.0
```
