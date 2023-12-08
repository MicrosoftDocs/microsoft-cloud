---
title: How to work with large datasets?
description: This article provides some guidance on how to work with large datasets in your applications.
author: waldekmastykarz
ms.author: wmastyka
ms.date: 12/18/2023
ms.topic: concept-article
ms.service: microsoft-cloud-for-developers

categories:
  - developer-tools
products:
  - microsoft-365
  - microsoft-graph
  - sharepoint-online
  - m365
ms.custom:
  - fcp
  - team=cloud_advocates
  - tool=devproxy
---

# How to work with large datasets?

When working with APIs that expose large data sets, use techniques such as pagination, filtering, sorting, and caching to optimize API requests for performance.

## Use pagination

Different APIs have different pagination options, so you should read the API's documentation to understand how to use pagination correctly. Some common pagination options include:

- specifying the number of records to retrieve per page,
- using a cursor to retrieve the next set of records,
- using a continuation token to retrieve the next page of data.

One important property you should keep in mind when using pagination is `@odata.nextLink`. This property contains a URL that the API returns when there are more results available than can be returned in a single response. You can use this property to retrieve the next page of results by making another API call to the URL specified in the `@odata.nextLink` property. You can continue to retrieve more pages of results until the `@odata.nextLink` property is no longer present in the response, indicating that you retrieved all results.

You can use Dev Proxy to see if your application uses pagination correctly.

## Set a reasonable page size

When specifying the number of records to retrieve per page, choose a reasonable page size. If the page size is too small, you have to make many API requests to retrieve all the data, which can be slow and inefficient. If the page size is too large, you put too much load on the API, which could lead to timeouts or errors. Choose a page size that balances the number of API requests with the amount of data retrieved per request.

## Cache paginated data

If you're retrieving paginated data that doesn't change frequently, consider [caching](./what-is-caching.md) the data to reduce the number of API requests and improve the performance of your application. Be sure to implement a cache invalidation strategy to ensure that the data in the cache is up to date.

By following these tips, you can work with APIs that expose large data sets and optimize your API requests for performance.

> [!div class="nextstepaction"]
> [Check if your application uses pagination correctly](../technical-reference/odatapagingguidanceplugin.md)
