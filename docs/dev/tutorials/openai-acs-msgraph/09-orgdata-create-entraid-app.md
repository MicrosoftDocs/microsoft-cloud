---
title: Creating a Microsoft Entra ID App Registration
description: Learn how to create a Microsoft Entra ID app registration to securely access Microsoft Graph APIs and integrate organizational data into your Line of Business application.
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

#customer intent: As a developer, I want to integrate Azure OpenAI, Azure Communication Services, and Microsoft Graph/Microsoft Graph Toolkit into a Line of Business application.

---

<!-- markdownlint-disable MD041 -->

# Organizational Data: Creating a Microsoft Entra ID App Registration

Enhance user productivity by integrating organizational data (emails, files, chats, and calendar events) directly into your custom applications. By using Microsoft Graph APIs and Microsoft Entra ID, you can seamlessly retrieve and display relevant data within your apps, reducing the need for users to switch context. Whether it's referencing an email sent to a customer, reviewing a Teams message, or accessing a file, users can quickly find the information they need without leaving your app, streamlining their decision-making process.

In this exercise, you will:

- Create a Microsoft Entra ID app registration so that Microsoft Graph can access organizational data and bring it into the app.
- Locate `team` and `channel` Ids from Microsoft Teams that are needed to send chat messages to a specific channel.
- Update the project's *.env* file with values from your Microsoft Entra ID app registration.

:::image type="content" source="./media/scenario-overview-no-title.png" alt-text="Microsoft Cloud scenario overview" border="false":::

### Create a Microsoft Entra ID App Registration

1. Go to [Azure portal](https://portal.azure.com) and select **Microsoft Entra ID**.
1. Select **Manage** --> **App registrations** followed by **+ New registration**.
1. Fill in the new app registration form details as shown below and select **Register**:
    - Name: *microsoft-graph-app*
    - Supported account types: *Accounts in any organizational directory (Any Microsoft Entra ID tenant - Multitenant)*
    - Redirect URI: 
        - Select **Single-page application (SPA)** and enter `http://localhost:4200` in the **Redirect URI** field.
    - Select **Register** to create the app registration.

    :::image type="content" source="./media/entraid-app-registration.png" alt-text="Microsoft Entra ID app registration form" border="true":::

<!-- 
1. After the app is registered, select **API permissions** in the Resource menu, locate the **Configured permissions** section, and select **+ Add a permission**.
1. Select **Microsoft Graph** followed by **Delegated permissions**.
1. In the **Select permissions** input enter `Chat.ReadWrite`, expand the **Chat** node, select the **Chat.ReadWrite** permission.
1. Go back to the **Select permissions** input and enter `Files.Read.All`, expand the **Files** node, select the **Files.Read.All** permission.
1. Select **Add permissions** at the bottom of the panel to add the permissions to the app. 
-->
1. Select **Overview** in the resource menu and copy the `Application (client) ID` value to your clipboard.

    :::image type="content" source="./media/entraid-client-id.png" alt-text="Microsoft Entra ID app client ID" border="true":::

### Update the Project's *.env* File

1. Open the *.env* file in your editor and assign the `Application (client) ID` value to `ENTRAID_CLIENT_ID`.

    ```
    ENTRAID_CLIENT_ID=<APPLICATION_CLIENT_ID_VALUE>
    ```

1. If you'd like to enable the ability to send a message from the app into a Teams Channel, sign in to [Microsoft Teams](https://teams.microsoft.com) using your Microsoft 365 dev tenant account (this is mentioned in the pre-reqs for the tutorial).

1. Once you're signed in, expand a team, and find a channel that you want to send messages to from the app. For example, you might select the **Company** team and the **General** channel (or whatever team/channel you'd like to use).

    :::image type="content" source="./media/teams-team-channel-get-link.png" alt-text="Get link to Teams channel" border="true":::

1. In the team header, click on the three dots (...) and select **Get link to team**.

1. In the link that appears in the popup window, the team ID is the string of letters and numbers after `team/`. For example, in the link "https://teams.microsoft.com/l/team/19%3ae9b9.../", the team ID is *19%3ae9b9...* up to the following `/` character. 

1. Copy the team ID and assign it to `TEAM_ID` in the *.env* file.

1. In the channel header, click on the three dots (...) and select **Get link to channel**.

1. In the link that appears in the popup window, the channel ID is the string of letters and numbers after `channel/`. For example, in the link "https://teams.microsoft.com/l/channel/19%3aQK02.../", the channel ID is *19%3aQK02...* up to the following `/` character.

1. Copy the channel ID and assign it to `CHANNEL_ID` in the *.env* file.

1. Save the *env* file before continuing.

<a id="start-app-services"></a>
[!INCLUDE [Start-Restart-Services](./includes/start-restart-services.md)]

### Next Step

> [!div class="nextstepaction"]
> [Organizational Data: Signing In a User and Getting an Access Token](./10-orgdata-add-auth-mgt.md)