---
title: What is caching?
description: This article explains what caching is.
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/26/2024
---

# What is caching?

Caching is a technique that's used to improve the performance and efficiency of an application by temporarily storing frequently accessed data in a location that's faster to retrieve than its original source. Caching helps you reduce the time it takes to retrieve data and reduce the load on the original data source.

You can use several types of caching in your application:

- **Memory caching**: Store data in the application's memory, which is faster to access than retrieving it from a database or external API. You typically use memory caching for data that's frequently accessed and changes infrequently.
- **Disk caching**: Store data on the local disk of the server or client, which can be faster to access than retrieving it from a remote location. You typically use disk caching for larger data sets that might not fit in memory, or for data that you need to persist between application restarts.
- **Distributed caching**: Store data in a distributed cache, which is a cache that's shared between multiple servers or instances of an application. Distributed caching is useful for applications that are deployed across multiple servers because it allows data to be shared and accessed quickly between instances.
- **Content delivery network (CDN) caching**: Store data on a CDN, which is a network of servers that are distributed around the world. CDN caching is helpful when you need to deliver static content, such as images or videos. It allows the content to be served from a location that's closer to the user, which reduces latency and improves performance.

When you implement caching in an application, consider the trade-offs between performance and data consistency. Caching can improve performance by reducing the time it takes to retrieve data, but it can also introduce the risk of serving stale or outdated data. To mitigate this risk, consider using techniques such as cache invalidation or cache expiration. Cache invalidation removes data from the cache when it's updated. Cache expiration sets a time-to-live limit for cached data, after which it's automatically removed from the cache.

Caching is a powerful technique that can help you improve the performance and efficiency of your application by temporarily storing frequently accessed data in a faster location. After you implement these techniques, verify that your application handles caching properly by using Dev Proxy.

## Next step

> [!div class="nextstepaction"]
> [Verify caching by using the CachingGuidancePlugin](../technical-reference/cachingguidanceplugin.md)
