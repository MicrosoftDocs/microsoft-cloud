<!-- markdownlint-disable MD041 -->

In today's digital environment, users manage a wide array of organizational data, including emails, chats, files, calendar events, and more, often across multiple platforms. This can lead to frequent context shifts—switching between tasks or applications—which can disrupt focus and reduce productivity.

To enhance efficiency, there is growing interest in integrating organizational data directly into the applications users use daily. By unifying data from various sources into one interface, users can access and manage information more seamlessly, minimizing context shifts and improving productivity. Additionally, this integration provides valuable insights and context, enabling users to make informed decisions and work more effectively.

In this exercise, you will:

- Learn how to Microsoft Graph can be used to retrieve organizational data.
- Walk through code examples of retrieving organizational data using Microsoft Graph.

### Using the Organizational Data Feature

1. In a [previous exercise](/microsoft-cloud/dev/tutorials/openai-msgraph-acs/?tutorial-step=7) you created an app registration in Azure AD and started the application server and API server. You also updated the following values in the `.env` file.

    ```
    AAD_CLIENT_ID=<APPLICATION_CLIENT_ID_VALUE>
    TEAM_ID=<TEAMS_TEAM_ID>
    CHANNEL_ID=<TEAMS_CHANNEL_ID>
    ```

1. Go back to the browser (*http://localhost:4200*). If you haven't already signed in, select **Sign In** in the header, and sign in with a user from your Microsoft 365 Developer tenant.

    > [!TIP]
    > You can view the users in your Microsoft 365 tenant by going to the [Microsoft 365 admin center](https://admin.microsoft.com/Adminportal/Home#/users).

    > [!NOTE]
    > In addition to authenticating the user, the Microsoft Graph Toolkit's `mgt-login` component also retrieves an access token that can be used by Microsoft Graph to access files, chats, emails, and calendar events, and other organizational data. The access token contains the scopes (permissions) such as `User.Read`, `Calendars.Read`, and others that you saw earlier:
    >
    >   ```typescript
    >   Providers.globalProvider = new Msal2Provider({
    >       clientId: AAD_CLIENT_ID, // retrieved from .env file
    >       scopes: ['User.Read', 'Chat.ReadWrite', 'Calendars.Read', 'ChannelMessage.Read.All', 'ChannelMessage.Send', 'Files.Read.All', 'Mail.Read',]
    >   });
    >   ```

1. Select **View Related Content** for *Adatum Corporation* in the datagrid. This will use Microsoft Graph to retrieve related files, chats, emails, and calendar events. Once the data loads, it'll be displayed below the datagrid in a tabbed interface. You'll see the following tabs (note that you won't have any data at this point):

    :::image type="content" source="../media/display-org-data.png" alt-text="Displaying Organizational Data":::

1. Click on each of the tabs and notice that no data is displayed. Your Microsoft 365 tenant doesn't have any related organizational data loaded for *Adatum Corporation* at this point. To fix this, perform at least one of the following actions:

    - Add files:
        - Visit https://onedrive.com and login using your Microsoft 365 Developer tenant credentials.
        - Select **My files** in the left navigation.
        - Select **Upload** and then **Folder** from the menu.
        - Select the *openai-msgraph-acs/customer documents* folder from the project.
    - Add chat messages and calendar events:
        - Visit https://teams.microsoft.com and login using your Microsoft 365 Developer tenant credentials.
        - Select **Teams** in the left navigation.
        - Select a team and channel and then select **New conversation**.
        - Enter *New order placed for Adatum Corporation* and select the **Send** button.
        - Feel free to add additional messages that mention other companies used in the application such as *Adventure Works Cycles*, *Contoso Pharmaceuticals*, and *Tailwind Traders*.
        - Select **Calendar** in the left navigation.
        - Select **New meeting**.
        - Enter "Meet with Adatum Corporation about project schedule" for the title.
        - Select **Save**.
    - Add emails:
        - Visit https://outlook.com and login using your Microsoft 365 Developer tenant credentials.
        - Select **New mail**.
        - Enter your personal email address in the **To** field.
        - Enter *New order placed for Adatum Corporation* for the subject.
        - Enter *Please review the latest order.* for the body.
        - Select **Send**.

1. Go back to the browser and refresh the page. Select **View Related Content** again for the *Adatum Corporation* row. You should now see data displayed in the tabs depending upon the steps you performed in the previous step.

### Exploring the Organizational Data Code


