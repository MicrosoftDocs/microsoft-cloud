---
title: Use Dev Proxy in a Docker container
description: How to use Dev Proxy in Docker
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/03/2026
---

<!-- INTENT: Run Dev Proxy in Docker -->
<!-- SOLUTION: Use Dev Proxy Docker image or add to existing image -->
<!-- RESULT: Dev Proxy running in Docker container -->
<!-- PLUGINS: various -->
<!-- JOB: configure-proxy -->
<!-- TIME: 15 minutes -->

# Use Dev Proxy in a Docker container

> **At a glance**  
> **Goal:** Run Dev Proxy in Docker  
> **Time:** 15 minutes  
> **Plugins:** Various  
> **Prerequisites:** Docker installed

When using Dev Proxy, you can choose to run it directly on your machine or in a Docker container. Running Dev Proxy in Docker is a great way to isolate it from your local environment. It also offers you a consistent way of using Dev Proxy between your local machine and CI/CD environments.

## Use the Dev Proxy Docker image

For your convenience, we provide a ready to use Docker image with Dev Proxy. You can use it to run Dev Proxy in a Docker container. You can pull the image from the GitHub Container Registry, by using the following command:

```bash
docker pull ghcr.io/dotnet/dev-proxy:latest
```

> [!NOTE]
> To try the latest preview features, use the beta version of the Dev Proxy container.
>
> ```bash
> docker pull ghcr.io/dotnet/dev-proxy:beta
> ```

If you use Dev Proxy in a CI/CD pipeline, consider using a specific version of the image, instead of `latest` or `beta`. This way, you can ensure that your pipeline isn't affected by any breaking changes introduced in the latest version of Dev Proxy. For example, to use Dev Proxy version 0.26.0, use the following command:

```bash
docker pull ghcr.io/dotnet/dev-proxy:0.26.0
```

## Start the Dev Proxy container

To start the Dev Proxy container, use the following command:

```bash
docker run \
    # remove the container when it exits
    --rm \
    # run the container interactively
    -it \
    # map Dev Proxy ports to the host
    -p 8000:8000 -p 8897:8897 \
    # map volumes to the host. Create the `cert` folder in the current working directory before running the command
    -v ${PWD}:/config -v ${PWD}/cert:/home/devproxy/.config/dev-proxy/rootCert \
    # specify the image to use
    ghcr.io/dotnet/dev-proxy:0.26.0
```

After you run the command, Dev Proxy starts automatically and listens for traffic on port 8000. Since it's running in a Docker container, it doesn't automatically register as a system proxy on the host to intercept web requests. Instead, you need to manually configure the system proxy on your host or configure the proxy for your application.

By running the container interactively (using the `-it` options), you can control Dev Proxy from the command line. Interacting with Dev Proxy is helpful, for example, for starting and stopping recording, clearing the screen, etc. If you start the container in the background, you can still control Dev Proxy using the [Dev Proxy API](../technical-reference/proxy-api.md).

## Parameters

The Dev Proxy Docker contains several parameters that you can use to customize its behavior.

### Ports

The image exposes the following ports:

- **8000** - the port on which Dev Proxy listens for incoming traffic.
- **8897** - the port on which Dev Proxy exposes its [API](../technical-reference/proxy-api.md). You can use it to interact with Dev Proxy programmatically.

> [!IMPORTANT]
> Be sure to map both ports to your host, so that you can access Dev Proxy from your local machine and use [Dev Proxy Toolkit](https://aka.ms/devproxy/toolkit).

### Volumes

The image exposes the following volumes:

- **/config** - the current working directory from which the container starts Dev Proxy. If the folder you map contains a `devproxyrc.json` file, Dev Proxy automatically uses it to configure itself.
- **/home/devproxy/.config/dev-proxy/rootCert** - the folder in which Dev Proxy stores its root certificate. By mapping the volume to your host, you can easily access the root certificate and install it in your system or browser.

> [!TIP]
> Alternatively to mapping the root certificate to your host, you can use the Dev Proxy API to [download the public key of the root certificate](../technical-reference/proxy-api.md#get-proxyrootcertificateformatcrt) in the PEM (Privacy Enhanced Mail) format. To download the certificate, call the `GET http://127.0.0.1:8897/proxy/rootCertificate?format=crt` endpoint.

## Using Dev Proxy in Docker

When you start the Dev Proxy container, it automatically starts Dev Proxy listening for incoming traffic on port 8000.

### Default configuration

Dev Proxy looks for the `devproxyrc.json` file in the folder you map to the `/config` volume. If it finds the file, it uses it to configure itself. If it doesn't find the file, it uses the default configuration.

### Custom configuration

You can use custom configuration for Dev Proxy by creating the `devproxyrc.json` file in the folder you map to the `/config` volume. Alternatively, you can specify the configuration file using the `--config-file` parameter when starting Dev Proxy. For example, to use the `myconfig.json` file, use the following command:

```bash
docker run \
    # remove the container when it exits
    --rm \
    # run the container interactively
    -it \
    # map Dev Proxy ports to the host
    -p 8000:8000 -p 8897:8897 \
    # map volumes to the host. Create the `cert` folder in the current working directory before running the command
    -v ${PWD}:/config -v ${PWD}/cert:/home/devproxy/.config/dev-proxy/rootCert \
    # specify the image to use
    ghcr.io/dotnet/dev-proxy:0.26.0 \
    # specify the configuration file to use
    --config-file /config/myconfig.json
```

The configuration file you specify must be accessible from the container. If you use a relative path, it must be relative to the `/config` volume.

### Other options

If you want to use other options, you can specify them in the same way as you would when running Dev Proxy directly on your machine. For example, to specify the URLs to watch, use the following command:

```bash
docker run \
    # remove the container when it exits
    --rm \
    # run the container interactively
    -it \
    # map Dev Proxy ports to the host
    -p 8000:8000 -p 8897:8897 \
    # map volumes to the host. Create the `cert` folder in the current working directory before running the command
    -v ${PWD}:/config -v ${PWD}/cert:/home/devproxy/.config/dev-proxy/rootCert \
    # specify the image to use
    ghcr.io/dotnet/dev-proxy:0.26.0 \
    # specify the URLs to watch
    --urls-to-watch "https://example.com/*"
```

## See also

- [Use Dev Proxy with .NET applications running in Docker](./use-dev-proxy-with-dotnet-docker.md)
- [Use Dev Proxy with Node.js applications running in Docker](./use-dev-proxy-with-nodejs-docker.md)
- [Use Dev Proxy in CI/CD scenarios](./use-dev-proxy-in-ci-cd-overview.md)
