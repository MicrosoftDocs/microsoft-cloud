---
title: Change mocks file
description: How to specify which mocks file to use
author: garrytrinder
ms.author: garrytrinder
ms.date: 01/24/2024
---

# Change mocks file

By default, the [MockResponsePlugin](../technical-reference/mockresponseplugin.md) and [GraphMockResponsePlugin](../technical-reference/graphmockresponseplugin.md) Dev Proxy plugins looks for a file called `mocks.json` in the current working directory.

To use a file with a different name, use:

```sh
devproxy --mocks-file my-mocks.json
```

Alternatively, you can specify the mocks file in the [devproxyrc.json](../technical-reference/devproxyrc.md) configuration file.

```json
{
  "mocksPlugin": {
    "mocksFile": "mocks.json"
  }
}
```

Learn more about the `MockResponsePlugin`.

> [!div class="nextstepaction"]
> [MockResponsePlugin](../technical-reference/mockresponseplugin.md)
