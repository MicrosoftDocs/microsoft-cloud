---
title: Use Dev Proxy with WSL
description: Learn how to use Dev Proxy with Windows Subsystem for Linux
author: waldekmastykarz
ms.author: wmastyka
ms.date: 03/26/2026
ms.topic: how-to
---

<!-- INTENT: Use Dev Proxy with WSL (Windows Subsystem for Linux) -->
<!-- SOLUTION: Two approaches - proxy on Windows host or inside WSL -->
<!-- RESULT: Dev Proxy intercepts requests from WSL applications -->
<!-- JOB: intercept-requests -->
<!-- TIME: 15 minutes -->

# Use Dev Proxy with WSL

> **At a glance**  
> **Goal:** Use Dev Proxy with Windows Subsystem for Linux (WSL)  
> **Time:** 15 minutes  
> **Prerequisites:** [Set up Dev Proxy](../get-started/set-up.md), WSL installed

When developing applications in WSL (Windows Subsystem for Linux), you have two options for using Dev Proxy: running it on your Windows host or inside WSL. Both approaches have their advantages depending on your development workflow.

## Option 1: Dev Proxy on Windows host (recommended)

The simplest approach is to install and run Dev Proxy on Windows. WSL automatically uses the Windows system proxy, so applications running in WSL have their traffic intercepted by Dev Proxy without extra configuration.

### Set up Dev Proxy on Windows

1. [Install Dev Proxy on Windows](../get-started/set-up.md) using winget or manual installation.
1. Start Dev Proxy:

   ```console
   devproxy
   ```

1. Trust the certificate when prompted.

### Use Dev Proxy from WSL

When Dev Proxy runs as the system proxy on Windows (the default behavior), applications in WSL automatically route their traffic through it.

Start your application in WSL as you normally would. Dev Proxy intercepts HTTPS requests made by your application.

> [!IMPORTANT]
> **Restart WSL after starting or stopping Dev Proxy**
>
> WSL caches the system proxy settings. If you start Dev Proxy after WSL is already running, or if you stop and restart Dev Proxy, you need to restart WSL for the changes to take effect:
>
> ```console
> wsl --shutdown
> ```
>
> Then start a new WSL session.

### Trust the certificate in WSL

For Dev Proxy to intercept HTTPS traffic, applications in WSL need to trust the Dev Proxy certificate. You can either copy the certificate from Windows or export it using the Dev Proxy API.

#### Option A: Copy the certificate from Windows

1. In WSL, copy the certificate from Windows:

   ```bash
   cp /mnt/c/Users/<your-username>/.config/dev-proxy/rootCert.pfx ~/
   ```

1. Export and install the certificate:

   ```bash
   # Export the certificate
   openssl pkcs12 -in ~/rootCert.pfx -clcerts -nokeys -out dev-proxy-ca.crt -passin pass:""
   # Install the certificate
   sudo cp dev-proxy-ca.crt /usr/local/share/ca-certificates/
   # Update certificates
   sudo update-ca-certificates
   ```

#### Option B: Use the Dev Proxy API

While Dev Proxy is running on Windows, download and install the certificate:

```bash
# Download the certificate from Dev Proxy API
curl -o dev-proxy-ca.crt http://127.0.0.1:8897/proxy/rootCertificate?format=crt
# Install the certificate
sudo cp dev-proxy-ca.crt /usr/local/share/ca-certificates/
# Update certificates
sudo update-ca-certificates
```

> [!NOTE]
> Some applications might require extra configuration to trust the system certificate store. For Node.js applications, you might need to set `NODE_TLS_REJECT_UNAUTHORIZED=0` for development purposes. Disabling certificate validation isn't recommended for production code.

## Option 2: Dev Proxy inside WSL

If you prefer to run Dev Proxy entirely within WSL, install it using the Linux installation method and configure the proxy manually.

### Install Dev Proxy in WSL

Install Dev Proxy using the setup script:

```bash
bash -c "$(curl -sL https://aka.ms/devproxy/setup.sh)"
```

### Configure the proxy manually

Since WSL doesn't have a graphical desktop environment, Dev Proxy can't automatically register as the system proxy. Configure your applications to use the proxy manually.

1. Create a Dev Proxy configuration file with `asSystemProxy` set to `false`:

   ```json
   {
     "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.3.0/rc.schema.json",
     "asSystemProxy": false,
     "plugins": [
       {
         "name": "RetryAfterPlugin",
         "enabled": true,
         "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll"
       },
       {
         "name": "GenericRandomErrorPlugin",
         "enabled": true,
         "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
         "configSection": "genericRandomErrorPlugin"
       }
     ],
     "urlsToWatch": [
       "https://jsonplaceholder.typicode.com/*"
     ],
     "genericRandomErrorPlugin": {
       "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.3.0/genericrandomerrorplugin.schema.json",
       "errorsFile": "devproxy-errors.json"
     },
     "rate": 50,
     "logLevel": "information"
   }
   ```

1. Start Dev Proxy:

   ```bash
   devproxy
   ```

1. Trust the certificate:

   ```bash
   # Export Dev Proxy root certificate
   openssl pkcs12 -in ~/.config/dev-proxy/rootCert.pfx -clcerts -nokeys -out dev-proxy-ca.crt -passin pass:""
   # Install the certificate
   sudo cp dev-proxy-ca.crt /usr/local/share/ca-certificates/
   # Update certificates
   sudo update-ca-certificates
   ```

1. Configure your application to use the proxy. Set the `HTTPS_PROXY` environment variable:

   ```bash
   export HTTPS_PROXY=http://127.0.0.1:8000
   ```

   Or specify it when running your application:

   ```bash
   HTTPS_PROXY=http://127.0.0.1:8000 node app.js
   ```

> [!TIP]
> For more information on configuring the proxy for Node.js applications, see [Use Dev Proxy with Node.js applications](use-dev-proxy-with-nodejs.md).

## Comparison of approaches

| Feature | Windows host (Option 1) | Inside WSL (Option 2) |
|---------|-------------------------|-----------------------|
| Setup complexity | Simple | Moderate |
| Certificate management | One-time copy from Windows | Manual export |
| Proxy configuration | Automatic (system proxy) | Manual (`HTTPS_PROXY`) |
| WSL restart required | Yes, when toggling proxy | No |
| Works with all WSL apps | Yes | Depends on app proxy support |

For most scenarios, running Dev Proxy on Windows (Option 1) is the recommended approach due to its simplicity and automatic proxy configuration.

## Troubleshooting

### Requests aren't intercepted when using Windows host

If Dev Proxy on Windows isn't intercepting requests from WSL:

1. Restart WSL to refresh proxy settings:

   ```console
   wsl --shutdown
   ```

1. Verify Dev Proxy is running and registered as system proxy.
1. Check that the Dev Proxy certificate is trusted in WSL.

### Certificate errors

If you see certificate errors:

1. Ensure you installed the certificate in WSL following the steps in [Trust the certificate in WSL](#trust-the-certificate-in-wsl).
1. For Node.js applications, you might need to configure the application to use the proxy agent. See [Use Dev Proxy with Node.js applications](use-dev-proxy-with-nodejs.md).

### WSL networking issues

WSL 2 uses a virtualized network adapter. If you experience networking issues:

1. Ensure your Windows Firewall allows Dev Proxy.
1. Try using `localhost` instead of `127.0.0.1` when configuring the proxy address.

## Related content

- [Use Dev Proxy with Node.js applications](use-dev-proxy-with-nodejs.md)
- [Use Dev Proxy with .NET applications](use-dev-proxy-with-dotnet.md)
- [Use Dev Proxy in a Docker container](use-dev-proxy-in-docker-container.md)
