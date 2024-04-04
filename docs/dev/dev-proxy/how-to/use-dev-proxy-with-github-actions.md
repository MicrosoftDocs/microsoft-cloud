---
title: Use Dev Proxy with GitHub Actions
description: How to use Dev Proxy with GitHub Actions
author: estruyf
ms.author: wmastyka
ms.date: 03/28/2024
---

# Use Dev Proxy with GitHub Actions

Using Dev Proxy with GitHub Actions is a great way to test your applications in a controlled environment. If you want to use Dev Proxy with your GitHub Actions workflow, you need to include a couple of steps in your workflow file to be able to trust the Dev Proxy certificate.

> [!NOTE]
> In this example, we will make use of an Ubuntu runner for GitHub Actions.

## Install the Dev Proxy

The first step is to install the Dev Proxy on the runner. You can do this by adding the following steps to your workflow file:

```yaml
- name: Install Dev Proxy
  run: bash -c "$(curl -sL https://aka.ms/devproxy/setup.sh)"
```

## Run the Dev Proxy

After installing the Dev Proxy, you need to start it. You can do this by adding the following step to your workflow file:

```yaml
- name: Start Dev Proxy
  run: devproxy
```

## Trust the Dev Proxy certificate

Once the Dev Proxy is started, it will have generated a self-signed certificate. To trust this certificate, you need to add the following step to your workflow file:

```yaml
- name: Install the Dev Proxy's certificate
  timeout-minutes: 1
  run: |
    echo "Export the Dev Proxy's Root Certificate"
    openssl pkcs12 -in ~/.config/dev-proxy/rootCert.pfx -clcerts -nokeys -out dev-proxy-ca.crt -passin pass:""

    echo "Installing the Dev Proxy's Root Certificate"
    sudo cp dev-proxy-ca.crt /usr/local/share/ca-certificates/

    echo "Updating the CA certificates"
    sudo update-ca-certificates
    echo "Certificate trusted."

    # Set the system proxy settings (optional)
    echo "http_proxy=http://127.0.0.1:8000" >> $GITHUB_ENV
    echo "https_proxy=http://127.0.0.1:8000" >> $GITHUB_ENV    
```

## Test the Dev Proxy

With the Dev Proxy installed, started, and the certificate trusted, you can now run your application and see how it behaves with the Dev Proxy.

```yaml
- name: Testing the Dev Proxy
  run: |
    # Using the Dev Proxy
    curl -ix http://127.0.0.1:8000 https://jsonplaceholder.typicode.com/posts

    # Using the system proxy settings (when http_proxy and https_proxy are set)
    curl -i https://jsonplaceholder.typicode.com/posts
```

## Full workflow file

Here is the full workflow file with all the steps included:

```yaml
name: Ubuntu Dev Proxy

on:
  push:
    branches:
      - main
      - dev
  workflow_dispatch:

jobs:
  test:
    timeout-minutes: 60
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Install Dev Proxy
        run: bash -c "$(curl -sL https://aka.ms/devproxy/setup.sh)"

      - name: Run Dev Proxy
        run: ./devproxy/devproxy &

      - name: Install the Dev Proxy's certificate
        timeout-minutes: 1
        run: |
          echo "Export the Dev Proxy's Root Certificate"
          openssl pkcs12 -in ~/.config/dev-proxy/rootCert.pfx -clcerts -nokeys -out dev-proxy-ca.crt -passin pass:""

          echo "Installing the Dev Proxy's Root Certificate"
          sudo cp dev-proxy-ca.crt /usr/local/share/ca-certificates/

          echo "Updating the CA certificates"
          sudo update-ca-certificates
          echo "Certificate trusted."

          # Set the system proxy settings (optional)
          echo "http_proxy=http://127.0.0.1:8000" >> $GITHUB_ENV
          echo "https_proxy=http://127.0.0.1:8000" >> $GITHUB_ENV          

      - name: Testing the Dev Proxy
        run: |
          # Using the Dev Proxy
          curl -ix http://127.0.0.1:8000 https://jsonplaceholder.typicode.com/posts

          # Using the system proxy settings (when http_proxy and https_proxy are set)
          curl -i https://jsonplaceholder.typicode.com/posts    
```
