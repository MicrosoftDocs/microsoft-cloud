---
title: Create an Azure Communication Services Resource
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

#customer intent: As a developer, I want to create a Azure Communication Services resource so that I can enable audio/video calling in my custom application.
---

<!-- markdownlint-disable MD041 -->

# Create an Azure Communication Services Resource

In this exercise you'll create an Azure Communication Services (ACS) resource in the Azure portal.

:::image type="content" source="./media/1-acs-azure-portal.png" alt-text="ACS in the Azure portal":::

To get started, perform the following tasks:

1. Visit the [Azure portal](https://portal.azure.com) in your browser and sign in.

1. Type *communication services* in the **search bar** at the top of the page and select **Communication Services** from the options that appear.

    :::image type="content" source="./media/search-acs-portal.png" alt-text="ACS in the Azure portal":::

1. Select **Create** in the toolbar.

1. Perform the following tasks:
    - Select your Azure subscription.
    - Select the resource group to use (create a new one if one doesn't exist).
    - Enter an ACS resource name. It must be a unique value.
    - Select a data location.

1. Select **Review + Create** followed by **Create**.

1. Once your ACS resource is created, navigate to it, and select **Settings --> Identities & User Access Tokens**.

1. Select the **Voice and video calling (VOIP)** checkbox.

1. Select **Generate**.

1. Copy the **Identity** and **User Access token** values to a local file. You'll need the values later in this exercise.

    :::image type="content" source="./media/user-identity-token.png" alt-text="User identity and token" border="false":::

1. Select **Settings --> Keys** and copy the **Primary key** connection string value to the local file where you copied the user identity and token values.

1. To run the application you'll need a Teams meeting link. Go to [Microsoft Teams](https://teams.microsoft.com), sign in with your Microsoft 365 developer tenant, and select the **Calendar** option on the left.

    > [!TIP]
    > If you don't currently have a Microsoft 365 account, you can sign up for the [Microsoft 365 Developer Program](https://developer.microsoft.com/microsoft-365/dev-program) subscription. It's *free* for 90 days and will continually renew as long as you're using it for development activity. If you have a Visual Studio *Enterprise* or *Professional* subscription, both programs include a free Microsoft 365 [developer subscription](https://aka.ms/MyVisualStudioBenefits), active for the life of your Visual Studio subscription.

1. Select a any date/time on the calendar, add a title for the meeting, invite a user from your Microsoft 365 developer tenant, and select **Save**.

1. Select the new meeting you added in the calendar and copy the **Teams meeting link** that is displayed into the same file where you stored the ACS user identity, token, and connection string.

    :::image type="content" source="./media/teams-meeting-link.png" alt-text="Teams Meeting Join Link":::

1. Now that your ACS resource is setup and you have a Teams meeting join link, let's get the React application up and running.

## Next Step

> [!div class="nextstepaction"]
> [Integrate Azure Communication Services Calling into a React App](03-Integrate-ACS-Calling.md)
