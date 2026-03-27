---
title: Use Dev Proxy with Docker Compose
description: Learn how to use Dev Proxy with applications running in Docker containers using Docker Compose
author: waldekmastykarz
ms.author: wmastyka
ms.date: 03/26/2026
---

<!-- INTENT: Use Dev Proxy with Docker Compose -->
<!-- SOLUTION: Configure Dev Proxy and app containers with Docker Compose -->
<!-- RESULT: Dev Proxy intercepts requests from Dockerized apps -->
<!-- PLUGINS: various -->
<!-- JOB: intercept-requests -->
<!-- TIME: 20 minutes -->

# Use Dev Proxy with Docker Compose

> **At a glance**  
> **Goal:** Use Dev Proxy with Docker Compose  
> **Time:** 20 minutes  
> **Plugins:** Various  
> **Prerequisites:** [Set up Dev Proxy](../get-started/set-up.md), Docker and Docker Compose installed

When developing applications with Docker, you might want to use Dev Proxy to test how your app handles API errors, throttling, or to mock API responses. This article shows you how to use Dev Proxy with Docker Compose in two scenarios:

- Dev Proxy running on your host machine, app in a container
- Dev Proxy running in a container alongside your app container

## Scenario 1: Dev Proxy on host, app in container

In this scenario, you run Dev Proxy directly on your machine while your application runs in a Docker container. This approach is useful when you're actively developing and want to interact with Dev Proxy directly.

### Configure your application container

Create a `docker-compose.yaml` file that configures your application to use the Dev Proxy running on your host:

```yaml
services:
  app:
    build: .
    environment:
      # Point to Dev Proxy on the host machine
      - HTTPS_PROXY=http://host.docker.internal:8000
      - HTTP_PROXY=http://host.docker.internal:8000
      # For Node.js applications
      - NODE_TLS_REJECT_UNAUTHORIZED=0
    extra_hosts:
      # Ensure host.docker.internal resolves correctly on Linux
      - "host.docker.internal:host-gateway"
```

### Start Dev Proxy on your host

Before starting your application container, start Dev Proxy on your host machine. On macOS and Linux, bind Dev Proxy to all network interfaces:

```console
devproxy --ip-address 0.0.0.0
```

On Windows, you can start Dev Proxy normally:

```console
devproxy
```

### Handle TLS/SSL certificates

When your application makes HTTPS requests through Dev Proxy, the container needs to trust the Dev Proxy certificate. You have two options:

#### Option 1: Disable certificate validation (development only)

For quick testing, you can disable certificate validation in your application. For Node.js applications, set the `NODE_TLS_REJECT_UNAUTHORIZED=0` environment variable (already included in the previous example). For .NET applications, you can configure your HTTP client to skip certificate validation.

> [!CAUTION]
> Never disable certificate validation in production code.

#### Option 2: Trust the Dev Proxy certificate in the container

For a more production-like setup, install the Dev Proxy certificate in your container. First, export the certificate from your host, and add it to your Docker image. See [Use Dev Proxy with .NET applications running in Docker](./use-dev-proxy-with-dotnet-docker.md) for detailed steps.

### Start your application

Start your application container:

```console
docker compose up
```

Your application now routes all HTTP and HTTPS traffic through Dev Proxy running on your host.

## Scenario 2: Dev Proxy and app in separate containers

In this more advanced scenario, both Dev Proxy and your application run in Docker containers. This approach is useful for CI/CD pipelines or when you want a fully containerized development environment.

### Create the certificate volume

Both containers need access to the Dev Proxy certificate. Create a Docker volume to share it:

```console
docker volume create devproxy-cert
```

### Create the entrypoint script

Create an `entrypoint.sh` script that installs the Dev Proxy certificate when your application container starts:

```bash
#!/bin/bash
set -e

# Wait for the Dev Proxy certificate to be available
echo "Waiting for Dev Proxy certificate..."
while [ ! -f /cert/rootCert.pfx ]; do
  sleep 1
done

# Convert PFX to PEM format
openssl pkcs12 \
  -in /cert/rootCert.pfx \
  -out /usr/local/share/ca-certificates/dev-proxy-ca.crt \
  -clcerts \
  -nokeys \
  -passin pass:""

# Update system certificates
update-ca-certificates

echo "Dev Proxy certificate installed"

# Execute the main command
exec "$@"
```

Make the script executable:

```console
chmod +x entrypoint.sh
```

### Create the application Dockerfile

Create a `Dockerfile` for your application that includes the certificate installation prerequisites:

```dockerfile
# Example for a Node.js application
FROM node:20

# Install certificate tools
RUN apt-get update && apt-get install -y \
    openssl \
    ca-certificates \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app

# Copy application files
COPY package*.json ./
RUN npm install
COPY . .

# Copy entrypoint script
COPY entrypoint.sh /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]
CMD ["node", "index.js"]
```

### Create the Docker Compose configuration

Create a `docker-compose.yaml` file that defines both services:

```yaml
services:
  dev-proxy:
    image: ghcr.io/dotnet/dev-proxy:latest
    ports:
      - "8000:8000"
      - "8897:8897"
    volumes:
      # Mount your Dev Proxy configuration
      - ./devproxyrc.json:/config/devproxyrc.json
      # Share the certificate with the app container
      - devproxy-cert:/home/devproxy/.config/dev-proxy/rootCert
    # Generate the certificate on startup
    command: ["--install-cert"]
    stdin_open: true
    tty: true

  app:
    build: .
    depends_on:
      - dev-proxy
    environment:
      # Point to the Dev Proxy container
      - HTTPS_PROXY=http://dev-proxy:8000
      - HTTP_PROXY=http://dev-proxy:8000
    volumes:
      # Mount the certificate volume
      - devproxy-cert:/cert:ro

volumes:
  devproxy-cert:
```

### Create a Dev Proxy configuration file

Create a `devproxyrc.json` file in the same directory as your `docker-compose.yaml`:

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.3.0/rc.schema.json",
  "urlsToWatch": [
    "https://api.example.com/*",
    "https://graph.microsoft.com/*"
  ],
  "plugins": [
    {
      "name": "GenericRandomErrorPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll"
    }
  ]
}
```

### Start the containers

Start both containers with Docker Compose:

```console
docker compose up
```

Dev Proxy starts first and generates its certificate. Your application container waits for the certificate, installs it, and then starts your application. All HTTP and HTTPS requests from your application are now routed through Dev Proxy.

### Interact with Dev Proxy

Since Dev Proxy runs in a container, you can interact with it in several ways:

#### Using the terminal

Attach to the Dev Proxy container:

```console
docker attach <container-name>
```

#### Using the Dev Proxy API

The Dev Proxy API is exposed on port 8897. You can use it to control Dev Proxy programmatically:

```console
# Start recording
curl -X POST http://localhost:8897/proxy/recording/start

# Stop recording
curl -X POST http://localhost:8897/proxy/recording/stop
```

For more information, see [Dev Proxy API](../technical-reference/proxy-api.md).

## See also

- [Use Dev Proxy in a Docker container](./use-dev-proxy-in-docker-container.md)
- [Use Dev Proxy with .NET applications running in Docker](./use-dev-proxy-with-dotnet-docker.md)
- [Use Dev Proxy with Node.js applications running in Docker](./use-dev-proxy-with-nodejs-docker.md)
