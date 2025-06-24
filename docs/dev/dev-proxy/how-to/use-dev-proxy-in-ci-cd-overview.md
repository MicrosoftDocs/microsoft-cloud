---
title: Use Dev Proxy in CI/CD scenarios
description: How to use Dev Proxy in continuous integration and continuous deployment (CI/CD) scenarios
author: waldekmastykarz
ms.author: wmastyka
ms.date: 06/25/2024
---

# Use Dev Proxy in CI/CD scenarios

Using Dev Proxy in CI/CD scenarios is a great way to test your applications in a controlled environment. When you combine Dev Proxy with tests that generate API requests in your application, you can cover a wide range of scenarios: from ensuring that your app doesn't use shadow- or nonproduction APIs to checking that it uses minimal Microsoft Graph permissions. While the exact configuration steps vary depending on your CI/CD system, here are some general principles that you should follow.

## Configure your runner

When you use Dev Proxy on your local machine, it runs interactively, waiting for you to control it by pressing keys. When you run it in a CI/CD pipeline, you can't control it with keys. To instruct Dev Proxy that it should run non-interactively, set the `CI` environment to `1` or `true`.

> [!NOTE]
> Most CI/CD runners already have the `CI` environment variable set. However, if you're using a custom runner, you might need to set it manually.

When Dev Proxy detects the `CI` environment variable, it doesn't wait for key presses. You can then stop Dev Proxy gracefully by sending a `SIGINT` signal to the process. Closing Dev Proxy gracefully is necessary when you record requests and want Dev Proxy to analyze them using its reporting plugins. Without the `CI` environment variable, Dev Proxy would wait for you to press <kbd>Ctrl</kbd>+<kbd>C</kbd>. The only way to stop it would be to forcefully close its process using `kill -9` or `kill -KILL`, which stops Dev Proxy immediately and prevents it from analyzing the recorded requests.

## Install Dev Proxy

In most cases, your runner doesn't have Dev Proxy installed. So before you can use Dev Proxy, you need to install it. The [installation steps](../get-started.md#install-dev-proxy) depend on the operating system that your runner uses.

> [!TIP]
> To speed up your pipeline, consider caching the Dev Proxy installation folder. This way, you won't need to download the Dev Proxy every time you run your pipeline. For the exact steps, refer to the documentation of your CI/CD system.

When installing Dev Proxy in a CI/CD setup, you'll likely want to pin the version of Dev Proxy that you install. Pinning the version ensures that your pipeline uses the same version of Dev Proxy every time you run it. The exact steps to pin a version depend on the operating system of your runner. Here's an example of how you can pin the version of Dev Proxy on a Linux-based runner:

```bash
# install Dev Proxy v0.29.0
bash -c "$(curl -sL https://aka.ms/devproxy/setup.sh)" -- v0.29.0
```

## Run Dev Proxy from a script

When you run Dev Proxy in a CI/CD pipeline, you need to start it from a script. Starting Dev Proxy from a script is necessary, because to close Dev Proxy gracefully, you need to send a `SIGINT` signal to its process. Closing a process using `SIGINT` is possible only from a script with job control enabled. If you start Dev Proxy directly from your CI/CD system, the Dev Proxy process ignores the `SIGINT` signal, and keeps running.

Here's an example of a bash script with job control enabled that starts Dev Proxy:

```bash
# enable job control so that we can send SIGINT to Dev Proxy
set -m

# the rest of the script using Dev Proxy
```

## Run Dev Proxy

After installing Dev Proxy, you need to start it. When starting Dev Proxy in a CI/CD pipeline it's important, that you start it in the background. Otherwise, your pipeline is blocked until you stop Dev Proxy.

If you're using a Linux-based runner, you can start Dev Proxy in the background by adding `&` at the end of the command, for example `./devproxy/devproxy &`.

## Wait for Dev Proxy to start

When you start Dev Proxy in the background, your script continues executing immediately. However, Dev Proxy needs some time to start. To ensure that Dev Proxy is ready before you start running your tests, wait for it to start. To wait for Dev Proxy to start, log its output to a file, and then check if Dev Proxy is listening for web requests, for example:

```bash
# log file path
log_file=devproxy.log

# start Dev Proxy in the background
# log Dev Proxy output to the log file
# log stdout and stderr to the file
./devproxy/devproxy > $log_file 2>&1 &

# store the Dev Proxy process ID
proxy_pid=$!

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

## Record requests

If you want to analyze API requests issued by your app, start Dev Proxy in the recording mode. You can start Dev Proxy with recording, using the `--record` command-line argument, for example:

```bash
./devproxy/devproxy --record
```

## Stop Dev Proxy

After you're done with your tests, stop Dev Proxy, by sending a `SIGINT` signal to it. You can send the `SIGINT` signal using the `kill` command, for example:

```bash
kill -INT $proxy_pid
```

After you stop the Dev Proxy process, it can take a moment for it to fully close. This is especially the case, when you configure Dev Proxy to analyze recorded requests. To ensure that Dev Proxy finished processing the recorded requests, wait for the process to close, for example:

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

## Example script

Here's an example of a bash script that starts Dev Proxy, waits for it to start, runs tests, records requests, and stops Dev Proxy:

```bash
# enable job control so that we can send SIGINT to Dev Proxy
set -m

log_file=devproxy.log

echo "Starting Dev Proxy..."

# start Dev Proxy in the background
# log Dev Proxy output to the log file
# log stdout and stderr to the file
./devproxy/devproxy --record > $log_file 2>&1 &

proxy_pid=$!

# wait for init
echo "Waiting for Dev Proxy to start..."
while true; do
  if grep -q "Recording" $log_file; then
    break
  fi
  sleep 1
done

# From: https://www.eliostruyf.com/playwright-microsoft-dev-proxy-github-actions/
# setup certificates
echo "Export the Dev Proxy's Root Certificate"
openssl pkcs12 -in ~/.config/dev-proxy/rootCert.pfx -clcerts -nokeys -out dev-proxy-ca.crt -passin pass:""

echo "Installing certutil..."
sudo apt install libnss3-tools

echo "Adding certificate to the NSS database for Chromium..."
mkdir -p $HOME/.pki/nssdb
certutil --empty-password -d $HOME/.pki/nssdb -N 
certutil -d sql:$HOME/.pki/nssdb -A -t "CT,," -n dev-proxy-ca.crt -i dev-proxy-ca.crt
echo "Certificate trusted." 

echo "Running Playwright tests..."
npm test

# send SIGINT to Dev Proxy to close it gracefully
echo "Stopping Dev Proxy..."
kill -INT $proxy_pid

echo "Waiting for Dev Proxy to complete..."
while true; do
  if grep -q -e "DONE" -e "No requests to process" -e "An error occurred in a plugin" $log_file; then
    break
  fi
  sleep 1
done
```
