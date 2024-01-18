---
title: Why do I keep getting 429 responses
description: How to fix all requests failing with a 429 response
author: garrytrinder
ms.author: garrytrinder
ms.date: 01/18/2024
---

# Why do I keep getting 429 responses

If you have the failure rate at `100%`, then no request can ever succeed.

If you configure a lower failure rate, then a previously throttled request is passed to the API provided the `Retry-After` time period elapses.
