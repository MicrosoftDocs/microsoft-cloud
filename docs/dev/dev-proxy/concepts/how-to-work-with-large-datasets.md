---
title: How to Work with Large Datasets
description: This article provides guidance on how to work with large datasets in your applications.
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/26/2024
---

<!-- INTENT: Optimize API usage with large datasets (pagination, filtering, caching) -->

# How to work with large datasets

When you work with APIs that expose large datasets, use techniques such as pagination, filtering, sorting, and caching to optimize API requests for performance.

## Use pagination

Different APIs have different pagination options. Read the API's documentation to understand how to use pagination correctly. Some common pagination options include:

- Specify the number of records to retrieve per page.
- Use a cursor to retrieve the next set of records.
- Use a continuation token to retrieve the next page of data.

The `@odata.nextLink` property contains a URL that the API returns when there are more results available than can be returned in a single response. You can use this property to retrieve the next page of results by making another API call to the URL specified in the `@odata.nextLink` property. You can continue to retrieve more pages of results until the `@odata.nextLink` property is no longer present in the response, which indicates that you retrieved all results.

You can use Dev Proxy to see if your application uses pagination correctly.

## Set a reasonable page size

When you specify the number of records to retrieve per page, choose a reasonable page size. If the page size is too small, you have to make many API requests to retrieve all the data. This process is slow and inefficient. If the page size is too large, you put too much load on the API. This strain can lead to time-outs or errors. Choose a page size that balances the number of API requests with the amount of data retrieved per request.

## Cache paginated data

If you're retrieving paginated data that doesn't change frequently, consider [caching](./what-is-caching.md) the data. Caching reduces the number of API requests and improves the performance of your application. Implement a cache invalidation strategy to ensure that the data in the cache is up to date.

By following these tips, you can work with APIs that expose large datasets and optimize your API requests for performance.

## Next step

> [!div class="nextstepaction"]
> [Check if your application uses pagination correctly](../technical-reference/odatapagingguidanceplugin.md)
