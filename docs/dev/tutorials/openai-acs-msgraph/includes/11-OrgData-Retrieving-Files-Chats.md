<!-- markdownlint-disable MD041 -->

In today's digital environment, users work with a wide array of organizational data, including emails, chats, files, calendar events, and more. This can lead to frequent context shifts—switching between tasks or applications—which can disrupt focus and reduce productivity. For example, a user working on a project might need to switch from their current application to Outlook to find crucial details in an email or switch to OneDrive for Business to find a related file. This back-and-forth action disrupts focus and wastes time that could be better spent on the task at hand.

To enhance efficiency, you can integrate organizational data directly into the applications users use everyday. By bringing in organizational data to your applications, users can access and manage information more seamlessly, minimizing context shifts and improving productivity. Additionally, this integration provides valuable insights and context, enabling users to make informed decisions and work more effectively.

In this exercise, you will:

- Learn how the *mgt-search-results* web component in the Microsoft Graph Toolkit can be used to search for files.
- Learn how to call Microsoft Graph directly to retrieve files from OneDrive for Business and chat messages from Microsoft Teams.
- Understand how to send chat messages to Microsoft Teams channels using Microsoft Graph.

> [!NOTE]
> The *mgt-search-results* component is currently in preview and is subject to change. In addition to showing how *mgt-search-results* can be used, the exercises also show how to perform the same tasks using Microsoft Graph directly.

### Using the Organizational Data Feature

1. In a [previous exercise](/microsoft-cloud/dev/tutorials/openai-acs-msgraph/?tutorial-step=8) you created an app registration in Azure AD and started the application server and API server. You also updated the following values in the `.env` file.

    ```
    AAD_CLIENT_ID=<APPLICATION_CLIENT_ID_VALUE>
    TEAM_ID=<TEAMS_TEAM_ID>
    CHANNEL_ID=<TEAMS_CHANNEL_ID>
    ```

    Ensure you've completed the [previous exercise](/microsoft-cloud/dev/tutorials/openai-acs-msgraph/?tutorial-step=8) before continuing.

1. Go back to the browser (*http://localhost:4200*). If you haven't already signed in, select **Sign In** in the header, and sign in with a user from your Microsoft 365 Developer tenant.

    > [!NOTE]
    > In addition to authenticating the user, the *mgt-login* web component also retrieves an access token that can be used by Microsoft Graph to access files, chats, emails, calendar events, and other organizational data. The access token contains the scopes (permissions) such as `Chat.ReadWrite`, `Files.Read.All`, and others that you saw earlier:
    >
    >   ```typescript
    >   Providers.globalProvider = new Msal2Provider({
    >       clientId: AAD_CLIENT_ID, // retrieved from .env file
    >       scopes: ['User.Read', 'Presence.Read', 'Chat.ReadWrite', 'Calendars.Read', 
    >                'ChannelMessage.Read.All', 'ChannelMessage.Send', 'Files.Read.All', 'Mail.Read']
    >   });
    >   ```

1. Select **View Related Content** for the *Adatum Corporation* row in the datagrid. This will cause organizational data such as files, chats, emails, and calendar events to be retrieved using Microsoft Graph. Once the data loads, it'll be displayed below the datagrid in a tabbed interface. It's important to mention that you may not see any data at this point since you haven't added any files, chats, emails, or calendar events for the user in your Microsoft 365 developer tenant yet. Let's fix that in the next step.

    :::image type="content" source="../media/display-org-data.png" alt-text="Displaying Organizational Data":::

1. Your Microsoft 365 tenant may not have any related organizational data for *Adatum Corporation* at this stage. To add some sample data, perform at least one of the following actions:

    - Add files by visiting https://onedrive.com and signing in using your Microsoft 365 Developer tenant credentials.
        - Select **My files** in the left navigation.
        - Select **Upload** and then **Folder** from the menu.
        - Select the *openai-acs-msgraph/customer documents* folder from the project you cloned.

        :::image type="content" source="../media/add-files-ondrive.png" alt-text="Uploading a Folder":::

    - Add chat messages and calendar events by visiting https://teams.microsoft.com and signing in using your Microsoft 365 Developer tenant credentials.
        - Select **Teams** in the left navigation.
        - Select a team and channel.
        - Select **New conversation**.
        - Enter *New order placed for Adatum Corporation* and select the **Send** button.

            Feel free to add additional chat messages that mention other companies used in the application such as *Adventure Works Cycles*, *Contoso Pharmaceuticals*, and *Tailwind Traders*.

            :::image type="content" source="../media/add-chat-teams.png" alt-text="Adding a Chat Message into a Teams Channel":::

        - Select **Calendar** in the left navigation.
        - Select **New meeting**.
        - Enter "Meet with Adatum Corporation about project schedule" for the title and body.
        - Select **Save**.

            :::image type="content" source="../media/add-calendar-event-teams.png" alt-text="Adding a Calendar Event in Teams":::

    - Add emails by visiting https://outlook.com and signing in using your Microsoft 365 Developer tenant credentials.
        - Select **New mail**.
        - Enter your personal email address in the **To** field.
        - Enter *New order placed for Adatum Corporation* for the subject and anything you'd like for the body.
        - Select **Send**.

            :::image type="content" source="../media/add-email-outlook.png" alt-text="Adding an Email in Outlook":::

1. Go back to the application in the browser and refresh the page. Select **View Related Content** again for the *Adatum Corporation* row. You should now see data displayed in the tabs depending upon which tasks you performed in the previous step.

1. Let's explore the code that enables the organizational data feature in the application. To retrieve the data, the client-side portion of the application uses the access token retrieved by the *mgt-login* component you looked at earlier to make calls to Microsoft Graph APIs. 

### Exploring Files Search Code

[!INCLUDE [Note-Open-Files-VS-Code](./tip-open-files-vs-code.md)]

1. Let's start by looking at how file data is retrieved from OneDrive for Business. Open *files.component.html* and take a moment to look through the code. The full path to the file is *client/src/app/files/files.component.html*.

1. Locate the *mgt-search-results* component and note the following attributes:

    ```html
    <mgt-search-results 
        class="search-results" 
        entity-types="driveItem" 
        [queryString]="searchText"
        (dataChange)="dataChange($any($event))" 
    />
    ```

    The *mgt-search-results* component is part of the Microsoft Graph Toolkit and as the name implies, it's used to display search results from Microsoft Graph. The component uses the following features in this example:

    - The `class` attribute is used to specify that the `search-results` CSS class should be applied to the component.
    - The `entity-types` attribute is used to specify the type of data to search for. In this case, the value is `driveItem` which is used to search for files in OneDrive for Business. 
    - The `queryString` attribute is used to specify the search term. In this case, the value is bound to the `searchText` property which is passed to the *files* component when the user selects **View Related Content** for a row in the datagrid. The square brackets around `queryString` indicate that the property is bound to the `searchText` value.
    - The `dataChange` event fires when the search results change. In this case, a customer function named `dataChange()` is called in the *files* component and the event data is passed to the function. The parenthesis around `dataChange` indicate that the event is bound to the `dataChange()` function.
    - Since no custom template is supplied, the default template built into `mgt-search-results` is used to display the search results. 

        :::image type="content" source="../media/viewing-files.png" alt-text="View Files from OneDrive for Business":::

1. An alternative to using components such as *mgt-search-results*, is to call Microsoft Graph APIs directly using code. To see how that works, open the *graph.service.ts* file and locate the `searchFiles()` function. The full path to the file is *client/src/app/core/graph.service.ts*.

    - You'll notice that a `query` parameter is passed to the function. This is the search term that's passed as the user selects **View Related Content** for a row in the datagrid. If no search term is passed, an empty array is returned.

        ```typescript
        async searchFiles(query: string) {
            const files: DriveItem[] = [];
            if (!query) return files;

            ...
        }
        ```

    - A filter is then created that defines the type of search to perform. In this case the code is searching for files in OneDrive for Business so `driveItem` is used just as you saw earlier with the `mgt-search-results` component. This is the same as passing `driveItem` to `entity-types` in the *mgt-search-results* component that you saw earlier. The `query` parameter is then added to the `queryString` filter along with `ContentType:Document`.

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

    - A call is then made to the `/search/query` Microsoft Graph API using the `Providers.globalProvider.graph.client.api()` function. The `filter` object is passed to the `post()` function which sends the data to the API.

        ```typescript
        const searchResults = await Providers.globalProvider.graph.client.api('/search/query').post(filter);
        ```

    - The search results are then iterated through to locate `hits`. Each `hit` contains information about a document that was found. A property named `resource` contains the document metadata and is added to the `files` array.

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

1. Looking through this code you can see that the *mgt-search-results* web component you explored earlier does a lot of work for you and significantly reduces the amount of code you have to write! However, there may be scenarios where you want to call Microsoft Graph directly to have more control over the data that's sent to the API or how the results are processed.

1. Open the *files.component.ts* file and locate the `search()` function. The full path to the file is *client/src/app/files/files.component.ts*. 

    Although the body of this function is commented out due to the *mgt-search-results* component being used, the function could be used to make the call to Microsoft Graph when the user selects **View Related Content** for a row in the datagrid. The `search()` function calls `searchFiles()` in *graph.service.ts* and passes the `query` parameter to it (the name of the company in this example). The results of the search are then assigned to the `data` property of the component. 
    
    ```typescript
    override async search(query: string) {
        this.data = await this.graphService.searchFiles(query);
    }
    ```
    
    The files component can then use the `data` property to display the search results. You could handle this using custom HTML bindings or by using another Microsoft Graph Toolkit control named [`mgt-file-list`](/graph/toolkit/components/file-list?WT.mc_id=m365-94501-dwahlin). Here's an example of binding the `data` property to one of the component's properties named `files` and handling the `itemClick` event as the user interacts with a file.

    ```html
    <mgt-file-list (itemClick)="itemClick($any($event))" [files]="data"></mgt-file-list>
    ```

1. Whether you choose to use the *mgt-search-results* component shown earlier or write custom code to call Microsoft Graph will depend on your specific scenario. In this example, the *mgt-search-results* component is used to simplify the code and reduce the amount of work you have to do.

### Exploring Teams Chat Messages Search Code

1. Go back to *graph.service.ts* and locate the `searchChatMessages()` function. You'll see that it's similar to the `searchFiles()` function you looked at previously.

    - It posts filter data to Microsoft Graph's `/search/query` API and converts the results into an array of objects that have information about the `teamId`, `channelId`, and `messageId` that match the search term.
    - To retrieve the Teams channel messages, a second call is made to the `/teams/${chat.teamId}/channels/${chat.channelId}/messages/${chat.messageId}` API and the `teamId`, `channelId`, and `messageId` are passed. This returns the full message details.
    - Additional filtering tasks are performed and the resulting messages are returned from `searchChatMessages()` to the caller.

1. Open the *chats.component.ts* file and locate the `search()` function. The full path to the file is *client/src/app/chats/chats.component.ts*. The `search()` function calls `searchChatMessages()` in *graph.service.ts* and passes the `query` parameter to it. 

    ```typescript
    override async search(query: string) {
        this.data = await this.graphService.searchChatMessages(query);
    }
    ```

    The results of the search are assigned to the `data` property of the component and data binding is used to iterate through the results array and render the data. This example uses an *Angular Material card* component to display the search results.

    ```html
    <div *ngIf="data.length">
        <mat-card *ngFor="let chatMessage of data">
            <mat-card-header>
                <mat-card-title [innerHTML]="chatMessage.summary"></mat-card-title>
            </mat-card-header>
            <mat-card-actions>
                <a mat-stroked-button color="basic" [href]="chatMessage.webUrl" target="_blank">View Message</a>
            </mat-card-actions>
        </mat-card>
    </div>
    ```

    :::image type="content" source="../media/viewing-teams-chats.png" alt-text="View Teams Chats":::

### Sending a Message to a Microsoft Teams Channel

1. In addition to searching for Microsoft Teams chat messages, the application also allows a user to send messages to a Microsoft Teams channel. This can be done by calling the `/teams/${teamId}/channels/${channelId}/messages` endpoint of Microsoft Graph. 

    :::image type="content" source="../media/viewing-teams-chat-dialog.png" alt-text="Sending a Teams Chat Message to a Channel":::

1. In the following code you'll see that a URL is created that includes the `teamId` and `channelId` values. Environment variable values are used for the team ID and channel ID in this example but those values could be dynamically retrieved as well using Microsoft Graph. The `body` constant contains the message to send. A POST request is then made and the results are returned to the caller.

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

1. Leveraging this type of functionality in Microsoft Graph provides a great way to enhance user productivbity by allowing users to interact with Microsoft Teams directly from the application they're already using. 