---
title: What is an OpenAPI spec
description: What are OpenAPI spec and what are some of the benefits of having them
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/15/2024
---

# What is an OpenAPI spec

OpenAPI specs, formerly known as Swagger, describe various aspects of an API. An OpenAPI spec describes the API's endpoints, parameters, and responses. OpenAPI spec are written in YAML or JSON and are used by tools to generate documentation, test cases, and client libraries. By having an OpenAPI spec, API builders can ensure that their API is accurately described, more accessible, and easier to integrate across a wide range of applications and services.

Here's why you should consider having an OpenAPI spec for your API:

- **Document API in a standardized way.** Document API specification in a consistent and human-readable format.
- **Generate client SDK.** Use tools such as [Kiota](/openapi/kiota/overview) to automate generating of client libraries in various programming languages.
- **Mock API.** Create mock servers based on the API specification, which helps you during the early stages of development when the actual API isn't yet implemented.
- **Improve collaboration.** Provide different teams (frontend, backend, QA, etc.) a clear understanding of the API's capabilities and limitations, and helps new team members to quickly get up to speed.
- **Simplify testing and validation.** Automate validation of API requests and responses against the specification, which makes it easier to identify discrepancies.
- **Integrate with API management tools.** Easily integrate, deploy, and monitor your APIs with many API management tools and gateways, such as [Azure API Center](/azure/api-center/) and [Azure API Management](/azure/api-management/).
- **Simplify API gateway configuration.** Use OpenAPI spec to configure API gateways, automating tasks such as routing, transformations, and CORS settings.

By using OpenAPI specs, you can create APIs that are well-designed and consistently documented. They're also more maintainable and easier to use both internally and by external consumers.

If you don't have an OpenAPI spec for your API, you can use Dev Proxy to generate one from the intercepted requests and responses.

> [!div class="nextstepaction"]
> [Generate an OpenAPI spec](../how-to/generate-openapi-document.md)
