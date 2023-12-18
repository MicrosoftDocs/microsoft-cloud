---
title: Change mocks file
description: How to specify which mocks file to use
author: garrytrinder
ms.author: garrytrinder
ms.date: 11/03/2023
---

# Change mocks file

By default, Dev Proxy looks for a file called `responses.json` in the current working directory.

To tell proxy to use a file with a different name, use:

```sh
devproxy --mocks-file mock.json
```
