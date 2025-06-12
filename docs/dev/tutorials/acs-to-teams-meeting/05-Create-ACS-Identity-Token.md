---
title: Dynamically Create an Azure Communication Services Identity and Token
description: In this tutorial you'll learn how Azure Communication Services can be used in a custom React application to allow a user to make an audio/video call into a Microsoft Teams meeting. You'll learn about the different building blocks that can be used to make this scenario possible and be provided with hands-on steps to walk you through the different Microsoft Cloud services involved.
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

:::image type="content" source="./media/4-acs-identity-token.png" alt-text="Create ACS Identity and Token":::

# [C#](#tab/csharp)

[!INCLUDE [CSharp](./05-Create-ACS-Identity-Token-CS.md)]

# [TypeScript](#tab/typescript)

[!INCLUDE [TypeScript](./05-Create-ACS-Identity-Token-TS.md)]

## Next Step

> [!div class="nextstepaction"]
> [Deploy the App to Azure Functions and Azure Container Apps](07-Deploy-to-Azure-Container-Apps.md)

