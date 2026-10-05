---
title: What Is an OpenAPI specification?
description: What an OpenAPI specification is, why your API benefits from one (client SDKs, mock APIs, API gateways), and how to generate one from your app's real traffic when you don't have it.
author: waldekmastykarz
ms.author: wmastyka
ms.date: 10/03/2026
---

<!-- INTENT: Understand OpenAPI specifications for API documentation -->

# What is an OpenAPI spec?

OpenAPI Specification, formerly known as Swagger, describes various aspects of an API. An OpenAPI specification (spec) describes the API's endpoints, parameters, and responses. OpenAPI specs are written in YAML or JSON and are used by tools to generate documentation, test cases, and client libraries. By having an OpenAPI spec, API builders can ensure that their API is accurately described, more accessible, and easier to integrate across a wide range of applications and services.

Here's why you should consider having an OpenAPI spec for your API:

- **Document an API in a standardized way.** Document an API specification in a consistent and human-readable format.
- **Generate a client SDK.** Use tools such as [Kiota](/openapi/kiota/overview) to automate generating of client libraries in various programming languages.
- **Create a mock API.** Create mock servers based on the API specification, which helps you during the early stages of development when the actual API isn't yet implemented.
- **Improve collaboration.** Provide different teams (front end, back end, QA) with a clear understanding of the API's capabilities and limitations, which helps new team members to get caught up quickly.
- **Simplify testing and validation.** Automate validation of API requests and responses against the specification, which makes it easier to identify discrepancies.
- **Integrate with API management tools.** Easily integrate, deploy, and monitor your APIs with many API management tools and gateways, such as [Azure API Center](/azure/api-center/) and [Azure API Management](/azure/api-management/).
- **Simplify API gateway configuration.** Use OpenAPI specs to configure API gateways and automate tasks such as routing, transformations, and cross-origin resource sharing settings.

By using OpenAPI specs, you can create APIs that are well-designed and consistently documented. They're also more maintainable and easier to use both internally and by external consumers.

## Don't have an OpenAPI spec yet?

Writing a spec by hand for an API that already exists takes time, and it drifts from what the API really does. Another option is to record what the API returns and generate the spec from that.

| Approach | What you get | What to watch for |
|---|---|---|
| Write it by hand | Full control over descriptions and examples | It takes time, and it drifts from the real API |
| Generate it from code annotations | A spec that stays in sync with your code | You need access to the API's code, and the framework has to support it |
| Generate it from recorded traffic | A spec for any API you can call, including ones you don't own | It covers only the requests you recorded, so exercise the parts of the API you need |

[Dev Proxy](../overview.md?WT.mc_id=devproxy-learn-what-is-openapi-spec) records the requests and responses between your app and an API, and generates an OpenAPI spec from them. For the steps, see [Generate an OpenAPI spec](../how-to/generate-openapi-spec.md?WT.mc_id=devproxy-learn-what-is-openapi-spec).

## Next step

> [!div class="nextstepaction"]
> [Generate an OpenAPI spec](../how-to/generate-openapi-spec.md?WT.mc_id=devproxy-learn-what-is-openapi-spec)
