The dollar sign has special meaning in some shells and might need to be escaped.

In PowerShell, use a backtick:

```pwsh
Invoke-WebRequest -Method GET -Uri "https://graph.microsoft.com/v1.0/users/id/photo/`$value" -Proxy http://localhost:8000
```

In bash, wrap the URL in double quotes and add a backslash:

```sh
curl -ikx http://localhost:8000 "https://graph.microsoft.com/v1.0/users/id/photo/\$value"
```
