<!-- markdownlint-disable MD041 -->

In today's digital environment, users work with a wide array of organizational data, including emails, chats, files, calendar events, and more. This can lead to frequent context shifts—switching between tasks or applications—which can disrupt focus and reduce productivity. For example, a user working on a project might need to switch from their current application to Outlook to find crucial details in an email or switch to OneDrive for Business to find a file. This back-and-forth action disrupts focus and wastes time that could be better spent on the task at hand.

To enhance efficiency, you can integrate organizational data directly into the applications users use daily. By bringing in organizational data, users can access and manage information more seamlessly, minimizing context shifts and improving productivity. Additionally, this integration provides valuable insights and context, enabling users to make informed decisions and work more effectively.

In this exercise, you will:

- Learn how to Microsoft Graph can be used to retrieve organizational data.
- Walk through code examples of retrieving data using Microsoft Graph.
- Understand how to send chat messages to Microsoft Teams channels using Microsoft Graph.

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

1. Select **View Related Content** for the *Adatum Corporation* row in the datagrid. This will trigger Microsoft Graph to retrieve related files, chats, emails, and calendar events. Once the data loads, it'll be displayed below the datagrid in a tabbed interface. Note that you won't have any data at this point.

    :::image type="content" source="../media/display-org-data.png" alt-text="Displaying Organizational Data":::

1. Your Microsoft 365 tenant doesn't have any related organizational data for *Adatum Corporation* at this point. To fix this, perform at least one of the following actions:

    - Add files:
        - Visit https://onedrive.com and login using your Microsoft 365 Developer tenant credentials.
        - Select **My files** in the left navigation.
        - Select **Upload** and then **Folder** from the menu.
        - Select the *openai-msgraph-acs/customer documents* folder from the project you cloned.

        :::image type="content" source="../media/add-files-ondrive.png" alt-text="Uploading a Folder":::

    - Add chat messages and calendar events:
        - Visit https://teams.microsoft.com and login using your Microsoft 365 Developer tenant credentials.
        - Select **Teams** in the left navigation.
        - Select a team and channel and then select **New conversation**.
        - Enter *New order placed for Adatum Corporation* and select the **Send** button.
        - Feel free to add additional chat messages that mention other companies used in the application such as *Adventure Works Cycles*, *Contoso Pharmaceuticals*, and *Tailwind Traders*.

        :::image type="content" source="../media/add-chat-teams.png" alt-text="Adding a Chat Message into a Teams Channel":::

        - Select **Calendar** in the left navigation.
        - Select **New meeting**.
        - Enter "Meet with Adatum Corporation about project schedule" for the title and body.
        - Select **Save**.

        :::image type="content" source="../media/add-calendar-event-teams.png" alt-text="Adding a Calendar Event in Teams":::

    - Add emails:
        - Visit https://outlook.com and login using your Microsoft 365 Developer tenant credentials.
        - Select **New mail**.
        - Enter your personal email address in the **To** field.
        - Enter *New order placed for Adatum Corporation* for the subject.
        - Enter *Please review the latest order.* for the body.
        - Select **Send**.

        :::image type="content" source="../media/add-email-outlook.png" alt-text="Adding an Email in Outlook":::

1. Go back to the application in the browser and refresh the page. Select **View Related Content** again for the *Adatum Corporation* row. You should now see data displayed in the tabs depending upon which tasks you performed in the previous step.

1. Let's explore some of the key code that enables the organizational data feature in the sample application. To retrieve the data, the client-side portion of the application uses the access token retrieved by the `mgt-login` web component to make calls to Microsoft Graph APIs. If you're new to Microsoft Graph, you can learn more about it in the [Microsoft Graph Fundamentals](/training/paths/m365-msgraph-fundamentals/) learning path.

    > [!NOTE]
    > You can make Microsoft Graph calls from a custom API or server-side application as well. View the [following tutorial](/microsoft-cloud/dev/tutorials/acs-to-teams-meeting?tabs=bash) to see an example.

### Exploring File and Chat Search Code

[!INCLUDE [Note-Open-Files-VS-Code](./tip-open-files-vs-code.md)]

1. Open *graph.service.ts* and take a moment to look through the included functions. The full path to the file is *openai-msgraph-acs/client/src/app/core/graph.service.ts*. Key functions include:

    - `searchFiles()` - Searches files in OneDrive for Business.
    - `searchChats()` - Searches chat messages in Microsoft Teams.
    - `searchEmails()` - Searches email messages.
    - `searchAgendaEvents()` - Searches calendar events.
    - `sendTeamsChat()` - Sends a chat message to a Microsoft Teams channel.

1. Let's break down the functionality provided by the `searchFiles()` function.

    - A `query` parameter is passed to the function. This is the search term passed by the user as they select **View Related Content** for a row in the datagrid.

        ```typescript
        async searchFiles(query: string) {
            const files: DriveItem[] = [];
            if (!query) return files;

            ...
        }
        ```

    - A filter is created that defines the type of search to perform. In this case the code is searching for files in OneDrive for Business so `driveItem` is used. The `query` parameter is then added to the `queryString` filter along with `ContentType:Document`.

        ```typescript
        const filter = {
        "requests": [
            {
                "entityTypes": [
                    "driveItem"
                ],
                "query": {
                    "queryString": `${query} AND ContentType:Document`
                }
            }
        ]
        };
        ```

    - A call is made to the `/search/query` Microsoft Graph API using the `Providers.globalProvider.graph.client.api()` function. A POST request by calling the `post()` function and passing the `filter`.

        ```typescript
        const searchResults = await Providers.globalProvider.graph.client.api('/search/query').post(filter);
        ```

    - The search results are then iterated through. If there are "hits" (results), the document is added to the `files` array. 

        ```typescript
        if (searchResults.value.length !== 0) {
            for (const hitContainer of searchResults.value[0].hitsContainers) {
                if (hitContainer.hits) {
                for (const hit of hitContainer.hits) {
                    files.push(hit.resource);
                }
                }
            }
        }
        ```

    - The `files` array is then returned to the caller.

        ```typescript
        return files;
        ```

1. Open the *files.component.ts* file and locate the `search()` function. The full path to the file is *openai-msgraph-acs/client/src/app/files/files.component.ts*. 

    This function is called when the user selects **View Related Content** for a row in the datagrid. The `search()` function calls `searchFiles()` in *graph.service.ts* and passes the `query` parameter to it (the name of the company in this example). The results of the search are then assigned to the `data` property of the component. The *files.component.html* file then uses the `data` property to display the search results.

    ```typescript
    override async search(query: string) {
        this.data = await this.graphService.searchFiles(query);
    }
    ```

1. Go back to *graph.service.ts* and locate the `searchChats()` function . You'll see that it's similar to `searchFiles()`. 

    - It calls Microsoft Graph's `/search/query` API and converts the results into an array of objects that have information about the `teamId`, `channelId`, and `messageId` that match the search term.
    - To retrieve the channel messages, a call is made to  `/teams/${chat.teamId}/channels/${chat.channelId}/messages/${chat.messageId}` and the `teamId`, `channelId`, and `messageId` are passed. 
    - Additional filtering tasks are performed and the resulting messages are returned to the caller.

1. Open the *chats.component.ts* file and locate the `search()` function. The full path to the file is *openai-msgraph-acs/client/src/app/chats/chats.component.ts*. The `search()` function calls `searchChats()` in *graph.service.ts* and passes the `query` parameter to it. The results of the search are then assigned to the `data` property of the component. The *chats.component.html* file then uses the `data` property to display the search results.

    ```typescript
    override async search(query: string) {
        this.data = await this.graphService.searchChats(query);
    }
    ```

### Sending a Message to a Teams Channel

1. In addition to searching for Microsoft Teams chat messages, the application also allows the user to send messages to a Teams channel. This can be done by calling the `/teams/${teamId}/channels/${channelId}/messages` endpoint of Microsoft Graph. In the following code you'll see that a URL is created that includes the `teamId` and `channelId` values (environment values provide these values). The `body` variable contains the message to send. A POST request is then made and the results are returned to the caller.

    ```typescript
    async sendTeamsChat(message: string): Promise<TeamsDialogData> {
        if (!message) new Error('No message to send.');
        if (!TEAM_ID || !CHANNEL_ID) new Error('Team ID or Channel ID not set in environment variables. Please set TEAM_ID and CHANNEL_ID in the .env file.');

        const url = `https://graph.microsoft.com/v1.0/teams/${TEAM_ID}/channels/${CHANNEL_ID}/messages`;
        const body = {
            "body": {
                "contentType": "html",
                "content": message
            }
        };
        const response = await Providers.globalProvider.graph.client.api(url).post(body);
        return {
            id: response.id,
            teamId: response.channelIdentity.teamId,
            channelId: response.channelIdentity.channelId,
            message: response.body.content,
            webUrl: response.webUrl,
            title: 'Send Teams Chat'
        };
    }
    ```

### Exploring Email Search Code

1. Microsoft Graph provides an API to search email messages that is quite straightforward to use. Go back to *graph.service.ts* and locate the `searchEmails()` function. It creates a URL that can be used to call the `messages` endpoint of Microsoft Graph and embeds the `query` parameter in it. The code then makes a GET request and returns the results to the caller.

    ```typescript
    async searchEmails(query:string) {
        if (!query) return [];
        // The $search operator will search the subject, body, and sender fields automatically
        const url = `https://graph.microsoft.com/v1.0/me/messages?$search="${query}"&$select=subject,bodyPreview,from,toRecipients,receivedDateTime,webLink`;
        const response = await Providers.globalProvider.graph.client.api(url).get();
        return response.value;
    }
    ```

1. The emails component located in *emails.component.ts* then calls `searchEmails()` and displays the results.

    ```typescript
    override async search(query: string) {
        this.data = await this.graphService.searchEmails(query);
    }
    ```

### Exploring Calendar Event Search Code

1. As with the email search functionality, Microsoft Graph provides an API to search calendar events (agenda items) as well. Locate the `searchAgendaEvents()` function in *graph.service.ts*. It creates start and end date/time values that are used to define the time period to search. It then creates a URL that can be used to call the `events` endpoint of Microsoft Graph and includes the `query` parameter and start and end date/time variables. A GET request is then made and the results are returned to the caller.

    Looking at the URL, you'll see that it uses the `$filter` and `$orderby` query parameters to filter the results and order them by the `start/dateTime` property. The `$filter` parameter uses the `contains()` function to search the `subject` field for the `query` parameter value.

    ```typescript
    async searchAgendaEvents(query:string) {
        if (!query) return [];
        const startDateTime = new Date();
        const endDateTime = new Date(startDateTime.getTime() + (7 * 24 * 60 * 60 * 1000));
        const url = `/me/events?startdatetime=${startDateTime.toISOString()}&enddatetime=${endDateTime.toISOString()}&$filter=contains(subject,'${query}')&orderby=start/dateTime`;

        const response = await Providers.globalProvider.graph.client.api(url).get();
        return response.value;
    }
    ```

1. As with the previous components, the agenda component (*agenda.component.ts* file) calls `search()` and displays the results.

    ```typescript
    override async search(query: string) {
        this.data = await this.graphService.searchAgendaEvents(query);
    }
    ```