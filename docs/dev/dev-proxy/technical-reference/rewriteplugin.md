---
title: RewritePlugin
description: RewritePlugin reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 11/18/2024
---

# RewritePlugin

Rewrites requests.

:::image type="content" source="../media/rewrite-plugin.png" alt-text="Screenshot of a command prompt with Dev Proxy rewriting an incoming API request." lightbox="../media/rewrite-plugin.png":::

## Plugin instance definition

```json
{
  "name": "RewritePlugin",
  "enabled": true,
  "pluginPath": "~appFolder/plugins/dev-proxy-plugins.dll",
  "configSection": "rewritePlugin"
}
```

## Configuration example

```json
{
  "rewritePlugin": {
    "rewritesFile": "rewrites.json"
  }
}
```

## Configuration properties

| Property | Description | Default |
| -------- | ----------- | :-----: |
| `rewritesFile` | Path to the file containing rewrite definitions | `rewrites.json` |

## Command line options

None

## Rewrite file examples

Following are examples of rewrite rules.

### Rewrite all requests from HTTP to HTTPS

Rewrite all requests from HTTP to HTTPS. In this context, **all** means all requests configured with Dev Proxy or the RewritePlugin.

```json
{
  "rewrites": [
    {
      "in": {
        "url": "^http://(.*)"
      },
      "out": {
        "url": "https://$1"
      }
    }
  ]
}
```

## Mocks file properties

| Property  | Description | Required |
| --------- | ----------- | :------: |
| `rewrites` | Array of [rewrite objects](#rewrite-object) that defines the list of rewrite rules that the RewritePlugin applies to the requests it intercepts | yes |

### Rewrite object

Each rewrite rule has the following properties:

| Property | Description | Required |
| -------- | ----------- | :------: |
| `in` | [Rewrite pattern](#rewrite-pattern) to match the incoming request. | yes |
| `out` | [Rewrite pattern](#rewrite-pattern) to rewrite the request | yes |

#### Remarks

If the request that the RewritePlugin intercepts, doesn't match all properties defined in the **in** pattern, the plugin doesn't apply the rewrite rule to the request.

### Rewrite pattern

Each rewrite pattern has the following properties:

| Property | Description | Required | Default value | Sample value |
| -------- | ----------- | :------: | ------------- | ------------ |
| `url` | Regular expression that the plugin applies to the URL. | yes | | `^http://(.*)` |

#### Remarks

If you use capture groups in the regular expression in the **in** patterns, you can refer to them in the **out** pattern. For example, if you want to rewrite `http://example.com/foo` to `https://example.com/foo`, you can use the following rewrite rule:

```json
{
  "in": {
    "url": "^http://(.*)"
  },
  "out": {
    "url": "https://$1"
  }
}
```
