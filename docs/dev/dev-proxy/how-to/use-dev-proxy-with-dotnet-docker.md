---
title: Use Dev Proxy with .NET applications running in Docker
description: How to use Dev Proxy with .NET applications running in Docker containers
author: waldekmastykarz
ms.author: wmastyka
ms.date: 02/06/2024
---

# Use Dev Proxy with .NET applications running in Docker

If you run your .NET application in a Docker container and want to use Dev Proxy, there are several steps you need to follow to make it work.

## Configure proxy for your Docker container

Because your .NET app is running inside a Docker container and Dev Proxy is running on your host, you need to configure the proxy to point to the IP address of your computer (`http://192.0.2.13` in the following example). .NET uses the `HTTPS_PROXY` environment variable to configure the proxy for its HTTP client. To configure the variable for your Docker container, use the `--env, -e` option when starting the container:

```bash
docker run --rm -it -v $(pwd):/usr/src/app -e HTTPS_PROXY=http://192.0.2.13:8000 mcr.microsoft.com/dotnet/sdk:8.0 bash
```

> [!IMPORTANT]
> When you use Dev Proxy on macOS, you need to attach it to the `0.0.0.0` address to make it accessible from the Docker container. To configure the IP address for Dev Proxy, start it with the following command:
>
> ```bash
> devproxy --ip-address 0.0.0.0
> ```

## Handle SSL certificates

Dev Proxy uses its own SSL certificate to inspect HTTPS traffic intercepted from your application. When you install Dev Proxy on your computer, it generates a self-signed SSL certificate and adds it to the list of trusted certificates. However, when running your application in a Docker container, the container doesn't have access to the SSL certificate installed on your computer. There are two ways for you to handle SSL certificates when using Dev Proxy with .NET applications running in Docker containers.

### Configure Dev Proxy certificate in your Docker container

To allow Dev Proxy to inspect HTTPS requests, you can configure the Dev Proxy SSL certificate as trusted in your Docker container.

> [!IMPORTANT]
> Because Docker doesn't persist changes to a container after closing it, you'll need to repeat these steps each time you start the container. To avoid this, create a custom Docker image with the following steps included.

Start, by exporting the Dev Proxy certificate to PEM.

On macOS run:

```bash
# export Dev Proxy certificate
security find-certificate -c "Dev Proxy CA" -a -p > dev-proxy-ca.pem
# rename to .crt
mv dev-proxy-ca.pem dev-proxy-ca.crt
```

Next, copy the certificate to your Docker container. The easiest way to copy the certificate is to copy it to the project folder, which you mount to the container.

Then, in the Docker container, trust the certificate. If you're using the `mcr.microsoft.com/dotnet/sdk` image, you can use the following commands:

```bash
# change to the directory where your application is located
cd /usr/app/src
# copy the certificate to the trusted certificates directory
cp dev-proxy-ca.crt /usr/local/share/ca-certificates
# update the trusted certificates
update-ca-certificates
```

### Ignore SSL certificate validation in your .NET application

Another way to handle SSL certificates when using Dev Proxy with .NET applications running in Docker containers is to ignore SSL certificate validation in your application. This approach requires you to modify your application code.

In your application, add the following code to ignore SSL certificate validation:

```csharp
ServicePointManager.ServerCertificateValidationCallback = (sender, certificate, chain, sslPolicyErrors) => true;
```

> [!CAUTION]
> Be sure to remove this code before deploying your application to production.
