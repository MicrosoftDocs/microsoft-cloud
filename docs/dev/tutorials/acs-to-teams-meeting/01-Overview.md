---
title: Audio/Video Calling from a Custom App into a Teams Meeting
description: Learn how to use Azure Communication Services in a React app to enable audio/video calling into Microsoft Teams meetings. This tutorial provides an overview of the key components used in this integration.
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

#customer intent: As a developer, I want to integrate Azure Communication Services video calling into a React application.

---

<!-- markdownlint-disable MD041 -->

# Tutorial: Audio/Video Calling from a Custom App into a Teams Meeting

**Level**: Intermediate

In this tutorial, you'll learn how Azure Communication Services can be used in a custom React application to allow a user to make an audio/video call into a Microsoft Teams meeting. You'll learn about the different building blocks that can be used to make this scenario possible and be provided with hands-on steps to walk you through the different Microsoft Cloud services involved.

## What You'll Build in this Tutorial

>[!VIDEO https://www.youtube.com/embed/5-Na9fgpVM8]

## Overview of the Application Solution

![ACS Audio/Video Solution](./media/architecture-no-title.png "Scenario Architecture")

## Prerequisites

- [Node LTS](https://nodejs.org) - Node LTS is used for this project
- [git](/devops/develop/git/install-and-set-up-git)
- [Visual Studio Code](https://code.visualstudio.com/) or another IDE of your choice.
- [Azure Functions Extension for VS Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)
- [Azure Functions Core Tools](/azure/azure-functions/functions-run-local?tabs=linux%2Cisolated-process%2Cnode-v4%2Cpython-v2%2Chttp-trigger%2Ccontainer-apps&pivots=programming-language-csharp)
- [Azure subscription](https://azure.microsoft.com/free/search)
- [Microsoft 365 developer tenant](https://developer.microsoft.com/microsoft-365/dev-program)
- [GitHub account](https://github.com)
- [Visual Studio](https://visualstudio.microsoft.com) if using the C# version of the tutorial. Visual Studio Code can also be used if preferred.

## Technologies used in this tutorial include

- React
- Azure Communication Services
- Azure Functions
- Microsoft Graph
- Microsoft Teams

## Next Step

> [!div class="nextstepaction"]
> [Create an Azure Communication Services Resource](02-create-acs-resource.md)
