---
title: Use Dev Proxy in CI/CD scenarios
description: How to use Dev Proxy in continuous integration and continuous deployment (CI/CD) scenarios
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/03/2026
---

<!-- INTENT: Integrate Dev Proxy in CI/CD -->
<!-- SOLUTION: Run Dev Proxy in pipeline with headless mode -->
<!-- RESULT: Automated API testing in CI/CD pipeline -->
<!-- PLUGINS: various -->
<!-- JOB: automate-testing -->
<!-- TIME: 30 minutes -->

# Use Dev Proxy in CI/CD scenarios

> **At a glance**  
> **Goal:** Integrate Dev Proxy in CI/CD  
> **Time:** 30 minutes  
> **Plugins:** Various  
> **Prerequisites:** [Set up Dev Proxy](../get-started/set-up.md)

Using Dev Proxy in CI/CD scenarios is a great way to test your applications in a controlled environment. When you combine Dev Proxy with tests that generate API requests in your application, you can cover a wide range of scenarios: from ensuring that your app doesn't use shadow- or nonproduction APIs to checking that it uses minimal Microsoft Graph permissions. While the exact configuration steps vary depending on your CI/CD system, here are some general principles that you should follow.

In most cases, your runner doesn't have Dev Proxy installed. So before you can use Dev Proxy, you need to install it. The [installation steps](../get-started/set-up.md#install-dev-proxy) depend on the operating system that your runner uses.

> [!TIP]
> To speed up your pipeline, consider caching the Dev Proxy installation folder. This way, you don't need to download Dev Proxy every time you run your pipeline. For the exact steps, refer to the documentation of your CI/CD system.

When installing Dev Proxy in a CI/CD setup, you typically want to pin the version of Dev Proxy that you install. Pinning the version ensures that your pipeline uses the same version of Dev Proxy every time you run it. The exact steps to pin a version depend on the operating system of your runner. Here's an example of how you can pin the version of Dev Proxy on a Linux-based runner:

```bash
# install Dev Proxy v1.0.0
bash -c "$(curl -sL https://aka.ms/devproxy/setup.sh)" -- v1.0.0
```

## Start Dev Proxy

After installing Dev Proxy, you need to start it. When starting Dev Proxy in a CI/CD pipeline, it's important that you start it in the background. Otherwise, your pipeline is blocked until you stop Dev Proxy.

If you're using a Linux-based runner, you can start Dev Proxy in the background:

```bash
# start Dev Proxy in the background
./devproxy/devproxy &`.
```

## Trust the Dev Proxy root certificate

After you start Dev Proxy, you need to trust the Dev Proxy root certificate before sending requests, because Dev Proxy uses a self-signed root certificate.

When Dev Proxy starts, the `rootCert.pfx` certificate is created in the `~/.config/dev-proxy/rootCert` folder.

Depending on your permissions, you might need to create the `~/.config/dev-proxy/rootCert` folder before starting Dev Proxy:

```bash
# ensure the rootCert folder
echo "Ensuring the Dev Proxy rootCert folder"
mkdir -p ~/.config/dev-proxy/rootCert

# start Dev Proxy
echo "Starting Dev Proxy"
./devproxy/devproxy &
```

If you're using a Linux-based runner, you can install the Dev Proxy root certificate by copying it to the system's trusted CA certificates store:

```bash
# export the Dev Proxy's Root Certificate
echo "Exporting the Dev Proxy's Root Certificate"
openssl pkcs12 -in ~/.config/dev-proxy/rootCert.pfx -clcerts -nokeys -out dev-proxy-ca.crt -passin pass:""

# install root certificate
echo "Installing the Dev Proxy's Root Certificate"
sudo cp dev-proxy-ca.crt /usr/local/share/ca-certificates/

# update CA certificates
echo "Updating the CA certificates"
sudo update-ca-certificates
```

## Wait for Dev Proxy to start

When you start Dev Proxy in the background, your script executes immediately. However, Dev Proxy needs some time to start. To ensure that Dev Proxy is ready before you start issuing requests, wait for it to start. To wait for Dev Proxy to start, log its output to a file, and then check if Dev Proxy is listening for web requests, for example:

```bash
# log file path
log_file=devproxy.log

# start Dev Proxy in the background
# log Dev Proxy output to the log file
# log stdout and stderr to the file
./devproxy/devproxy > $log_file 2>&1 &

# wait for init
echo "Waiting for Dev Proxy to start..."
while true; do
  if grep -q "Listening on 127.0.0.1:8000" $log_file; then
    break
  fi
  sleep 1
done

# the rest of your script
```

## Configure proxy environment variables

Many applications and libraries use the `http_proxy` and `https_proxy` environment variables to determine the proxy server to use for HTTP and HTTPS requests. To ensure that your application uses Dev Proxy, you need to set these environment variables to point to the Dev Proxy instance.

```bash
export http_proxy=http://127.0.0.1:8000
export https_proxy=http://127.0.0.1:8000
```

## Control Dev Proxy

When you run Dev Proxy in a CI/CD pipeline, you can't control it interactively. Instead, you can control it by sending requests to the [Dev Proxy API](../technical-reference/proxy-api.md).

If you want to analyze API requests issued by your app, you can send a `POST` request to the `/record` endpoint:

```bash
curl -X POST http://localhost:8897/proxy/record -H "Content-Type: application/json" -d '{"recording": true}'
```

To stop Dev Proxy, you can send a `POST` request to the `/stop` endpoint:

```bash
curl -X POST http://localhost:8897/proxy/stop
```

After you stop the Dev Proxy process, it can take a moment for it to fully close. To ensure that Dev Proxy finished, wait for the process to close, for example:

```bash
echo "Waiting for Dev Proxy to complete..."
while true; do
  if grep -q -e "DONE" -e "No requests to process" -e "An error occurred in a plugin" $log_file; then
    break
  fi
  sleep 1
done
```

When all recording plugins finish running, Dev Proxy prints the `DONE` message to the output. If there were no requests to process, Dev Proxy prints the `No requests to process` message. If an error occurred in a plugin, Dev Proxy prints the `An error occurred in a plugin` message. When you see any of these messages, you can be sure that Dev Proxy finished processing the recorded requests.

## Example start script

Here's an example of a bash script that you can use to start Dev Proxy in a CI/CD pipeline:

```bash
log_file=devproxy.log

echo "Ensuring Dev Proxy rootCert folder"
mkdir -p ~/.config/dev-proxy/rootCert

# start Dev Proxy in the background
# log Dev Proxy output to the log file
# log stdout and stderr to the file
echo "Starting Dev Proxy"
./devproxy/devproxy > $log_file 2>&1 &

echo "Waiting for Dev Proxy to start..."
while true; do
  if grep -q "Listening on 127.0.0.1:8000" $log_file; then
    break
  fi
  sleep 1
done

echo "Exporting the Dev Proxy's Root Certificate"
openssl pkcs12 -in ~/.config/dev-proxy/rootCert.pfx -clcerts -nokeys -out dev-proxy-ca.crt -passin pass:""

echo "Installing the Dev Proxy's Root Certificate"
sudo cp dev-proxy-ca.crt /usr/local/share/ca-certificates/

echo "Updating the CA certificates"
sudo update-ca-certificates

echo "Set proxy variables"
export http_proxy=http://127.0.0.1:8000
export https_proxy=http://127.0.0.1:8000
```

## See also

- [Use Dev Proxy with GitHub Actions](./use-dev-proxy-with-github-actions.md)
- [Use Dev Proxy with Azure Pipelines](./use-dev-proxy-with-azure-pipelines.md)
- [Dev Proxy API](../technical-reference/proxy-api.md)
