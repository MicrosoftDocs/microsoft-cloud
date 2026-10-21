---
title: Change mocks file
description: How to specify which mocks file to use
author: garrytrinder
ms.author: garrytrinder
ms.date: 10/21/2026
---

<!-- INTENT: Use a different mocks file location -->
<!-- SOLUTION: Set mocksFile in MockResponsePlugin config section -->
<!-- RESULT: Dev Proxy loads mocks from custom file path -->
<!-- PLUGINS: MockResponsePlugin, GraphMockResponsePlugin -->
<!-- JOB: mock-api -->
<!-- TIME: 3 minutes -->

# Change mocks file

> **At a glance**  
> **Goal:** Specify a custom mocks file for the MockResponsePlugin  
> **Time:** 3 minutes  
> **Plugins:** [MockResponsePlugin](../technical-reference/mockresponseplugin.md), [GraphMockResponsePlugin](../technical-reference/graphmockresponseplugin.md)  
> **Prerequisites:** [Set up Dev Proxy](../get-started/set-up.md)

By default, the [MockResponsePlugin](../technical-reference/mockresponseplugin.md) and [GraphMockResponsePlugin](../technical-reference/graphmockresponseplugin.md) Dev Proxy plugins look for a file called `mocks.json` in the same folder as the Dev Proxy configuration file.

To use a file with a different name, use:

```console
devproxy --mocks-file my-mocks.json
```

Dev Proxy resolves the path that you pass with `--mocks-file` relative to the configuration file, not the current working directory. For more information, see [Resolve relative paths](../technical-reference/resolve-relative-paths.md).

Alternatively, you can specify the mocks file in the [devproxyrc.json](../technical-reference/devproxyrc.md) configuration file. Dev Proxy automatically reloads its configuration when you save changes, so you don't need to restart the proxy.

**File:** devproxyrc.json

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

## See also

- [Mock responses](./mock-responses.md) - Complete mocking guide
- [MockResponsePlugin](../technical-reference/mockresponseplugin.md) - Full reference
- [Glossary](../concepts/glossary.md) - Dev Proxy terminology
