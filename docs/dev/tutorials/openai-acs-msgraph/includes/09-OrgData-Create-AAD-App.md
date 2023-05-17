<!-- markdownlint-disable MD041 -->

Enhance user productivity by integrating organizational data (emails, files, chats, and calendar events) directly into your custom applications. By using Microsoft Graph APIs and Azure Active Directory, you can seamlessly retrieve and display relevant data within your apps, reducing the need for users to switch context. Whether it's referencing an email sent to a customer, reviewing a Teams message, or accessing a file, users can quickly find the information they need without leaving your app, streamlining their decision-making process.

In this exercise, you will:

- Create an Azure Active Directory (Azure AD) app registration so that Microsoft Graph can access organizational data and bring it into the app.
- Locate `team` and `channel` Ids from Microsoft Teams that are needed to send chat messages to a specific channel.
- Update the project's *.env* file with values from your Azure AD app registration.

:::image type="content" source="../media/scenario-overview-no-title.png" alt-text="Microsoft Cloud scenario overview" border="false":::

### Create an Azure Active Directory App Registration

1. Go to [Azure Portal](https://portal.azure.com) and select **Azure Active Directory**.
1. Select the **App registration** tab followed by **+ New registration**.
1. Fill in the new app registration form details as shown below and select **Register**:
    - Name: *microsoft-graph-app*
    - Supported account types: *Accounts in any organizational directory (Any Azure AD directory - Multitenant)*
    - Redirect URI: 
        - Select **Single-page application (SPA)** and enter `http://localhost:4200` in the **Redirect URI** field.
    - Select **Register** to create the app registration.

    :::image type="content" source="../media/aad-app-registration.png" alt-text="Azure AD app registration form" border="true":::

<!-- 
1. After the app is registered, select **API permissions** in the Resource menu, locate the **Configured permissions** section, and select **+ Add a permission**.
1. Select **Microsoft Graph** followed by **Delegated permissions**.
1. In the **Select permissions** input enter `Chat.ReadWrite`, expand the **Chat** node, select the **Chat.ReadWrite** permission.
1. Go back to the **Select permissions** input and enter `Files.Read.All`, expand the **Files** node, select the **Files.Read.All** permission.
1. Select **Add permissions** at the bottom of the panel to add the permissions to the app. 
-->
1. Select **Overview** in the Resource menu and copy the `Application (client) ID` value to your clipboard.

    :::image type="content" source="../media/aad-client-id.png" alt-text="Azure AD app client ID" border="true":::

### Update the Project's *.env* File

1. Open the *.env* file in your editor and assign the `Application (client) ID` value to `AAD_CLIENT_ID`.

    ```
    AAD_CLIENT_ID=<APPLICATION_CLIENT_ID_VALUE>
    ```

1. If you'd like to enable the ability to send a message from the app into a Teams Channel, sign in to [Microsoft Teams](https://teams.microsoft.com) using your Microsoft 365 dev tenant account.

1. Once you're signed in, expand a team, and find a channel that you want to send messages to from the app. For example, you might select the **Company** team and the **General** channel (or whatever team/channel you'd like to use).

    :::image type="content" source="../media/teams-team-channel-get-link.png" alt-text="Get link to Teams channel" border="true":::

1. In the team header, click on the three dots (...) and select `Get link to team`.

1. In the link that appears in the popup window, the team ID is the string of letters and numbers after `team/`. For example, in the link "https://teams.microsoft.com/l/team/19%3ae9b9.../", the team ID is *19%3ae9b9...* up to the following `/` character. 

1. Copy the team ID and assign it to `TEAM_ID` in the *.env* file.

1. In the channel header, click on the three dots (...) and select `Get link to channel`.

1. In the link that appears in the popup window, the channel ID is the string of letters and numbers after `channel/`. For example, in the link "https://teams.microsoft.com/l/channel/19%3aQK02.../", the channel ID is *19%3aQK02...* up to the following `/` character.

1. Copy the channel ID and assign it to `CHANNEL_ID` in the *.env* file.

1. Save the *env* file before continuing.

[!INCLUDE [Start-Restart-Services](./Start-Restart-Services.md)]