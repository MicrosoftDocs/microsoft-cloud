---
title: Dynamically Create an Azure Communication Services Identity and Token
description: Learn how to dynamically generate Azure Communication Services user identities and tokens using Azure Functions. This enables secure user authentication for audio/video calling.
author: DanWahlin
ms.author: dwahlin
ms.date: 06/12/2025
ms.topic: tutorial
ms.service: microsoft-cloud-for-developers

categories:
  - developer-tools
products:
  - azure
  - github
ms.custom:
  - fcp
  - team=cloud_advocates

#customer intent: As a developer, I want to dynamically create an Azure Communication Services identity and token.
---

<!-- markdownlint-disable MD041 -->

# Dynamically Create an Azure Communication Services Identity and Token

In this exercise you'll learn how to dynamically retrieve user identity and token values from Azure Communication Services using Azure Functions. Once retrieved, the values will be passed to the ACS UI composite to enable a call to be made by a customer.

:::image type="content" source="../media/4-acs-id-tk.png" alt-text="Create ACS Identity and Token":::

# [C#](#tab/csharp)

[!INCLUDE [CSharp](./includes/05-create-acs-id-tk-cs.md)]

# [TypeScript](#tab/typescript)

[!INCLUDE [TypeScript](./includes/05-create-acs-id-tk-ts.md)]

## Next Step

> [!div class="nextstepaction"]
> [Deploy the App to Azure Functions and Azure Container Apps](./07-deploy-to-azure-container-apps.md)

