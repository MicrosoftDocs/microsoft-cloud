<!-- markdownlint-disable MD041 -->

In today's digital environment, users work with a wide array of organizational data, including emails, chats, files, calendar events, and more. This can lead to frequent context shifts—switching between tasks or applications—which can disrupt focus and reduce productivity. For example, a user working on a project might need to switch from their current application to Outlook to find crucial details in an email or switch to OneDrive for Business to find a file. This back-and-forth action disrupts focus and wastes time that could be better spent on the task at hand.

To enhance efficiency, you can integrate organizational data directly into the applications users use daily. By bringing in organizational data, users can access and manage information more seamlessly, minimizing context shifts and improving productivity. Additionally, this integration provides valuable insights and context, enabling users to make informed decisions and work more effectively.

In this exercise, you will:

- Learn how the *mgt-search-results* web component in the Microsoft Graph Toolkit can be used to search for organizational data.
- Learn how to call Microsoft Graph directly to retrieve organizational data.
- Understand how to send chat messages to Microsoft Teams channels using Microsoft Graph.

### Using the Organizational Data Feature

1. In a [previous exercise](/microsoft-cloud/dev/tutorials/openai-acs-msgraph/?tutorial-step=7) you created an app registration in Azure AD and started the application server and API server. You also updated the following values in the `.env` file.

    ```
    AAD_CLIENT_ID=<APPLICATION_CLIENT_ID_VALUE>
    TEAM_ID=<TEAMS_TEAM_ID>
    CHANNEL_ID=<TEAMS_CHANNEL_ID>
    ```

1. Go back to the browser (*http://localhost:4200*). If you haven't already signed in, select **Sign In** in the header, and sign in with a user from your Microsoft 365 Developer tenant.

    > [!TIP]
    > You can view the users in your Microsoft 365 tenant by going to the [Microsoft 365 admin center](https://admin.microsoft.com/Adminportal/Home#/users).

    > [!NOTE]
    > In addition to authenticating the user, the Microsoft Graph Toolkit's *mgt-login* web component also retrieves an access token that can be used by Microsoft Graph to access files, chats, emails, calendar events, and other organizational data. The access token contains the scopes (permissions) such as `User.Read`, `Calendars.Read`, and others that you saw earlier:
    >
    >   ```typescript
    >   Providers.globalProvider = new Msal2Provider({
    >       clientId: AAD_CLIENT_ID, // retrieved from .env file
    >       scopes: ['User.Read', 'Presence.Read', 'Chat.ReadWrite', 'Calendars.Read', 
    >                'ChannelMessage.Read.All', 'ChannelMessage.Send', 'Files.Read.All', 'Mail.Read']
    >   });
    >   ```

1. Select **View Related Content** for the *Adatum Corporation* row in the datagrid. This will cause organizational data such as files, chats, emails, and calendar events to be retrieved using Microsoft Graph. Once the data loads, it'll be displayed below the datagrid in a tabbed interface. Note that you probably won't see any data at this point since you haven't added any files, chats, emails, or calendar events for the user in your Microsoft 365 developer tenant yet. Let's fix that in the next step.

    :::image type="content" source="../media/display-org-data.png" alt-text="Displaying Organizational Data":::

1. Your Microsoft 365 tenant may not have any related organizational data for *Adatum Corporation* at this point. To add some sample data, perform at least one of the following actions:

    - Add files:
        - Visit https://onedrive.com and login using your Microsoft 365 Developer tenant credentials.
        - Select **My files** in the left navigation.
        - Select **Upload** and then **Folder** from the menu.
        - Select the *openai-acs-msgraph/customer documents* folder from the project you cloned.

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
        - Enter *New order placed for Adatum Corporation* for the subject and anything you'd like for the body.
        - Select **Send**.

        :::image type="content" source="../media/add-email-outlook.png" alt-text="Adding an Email in Outlook":::

1. Go back to the application in the browser and refresh the page. Select **View Related Content** again for the *Adatum Corporation* row. You should now see data displayed in the tabs depending upon which tasks you performed in the previous step.

1. Let's explore some of the code that enables the organizational data feature in the application. To retrieve the data, the client-side portion of the application uses the access token retrieved by the *mgt-login* web component you looked at earlier to make calls to Microsoft Graph APIs. 

### Exploring Files Search Code

[!INCLUDE [Note-Open-Files-VS-Code](./tip-open-files-vs-code.md)]

1. Let's start by looking at how file data is retrieved from OneDrive for Business. Open *files.component.html* and take a moment to look through the code. The full path to the file is *openai-acs-msgraph/client/src/app/files/files.component.html*.

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

1. An alternative to using components such as *mgt-search-results* is to call Microsoft Graph APIs directly using code. To see how that works, open the *graph.service.ts* file and locate the `searchFiles()` function. The full path to the file is *openai-acs-msgraph/client/src/app/core/graph.service.ts*.

    - You'll notice that a `query` parameter is passed to the function. This is the search term that's passed as the user selects **View Related Content** for a row in the datagrid. If no search term is passed, an empty array is returned.

        ```typescript
        async searchFiles(query: string) {
            const files: DriveItem[] = [];
            if (!query) return files;

            ...
        }
        ```

    - A filter is then created that defines the type of search to perform. In this case the code is searching for files in OneDrive for Business so `driveItem` is used. This is the same as passing `driveItem` to `entity-types` in the *mgt-search-results* component that you saw earlier. The `query` parameter is then added to the `queryString` filter along with `ContentType:Document`.

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

    - The search results are then iterated through to locate `hits`. Each `hit` contains information about the document that was found. A property named `resource` contains the document metadata and is added to the `files` array.

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

1. Open the *files.component.ts* file and locate the `search()` function. The full path to the file is *openai-acs-msgraph/client/src/app/files/files.component.ts*. 

    Although the body of this function is commented out due to the *mgt-search-results* component being used, the function could be used to make the call to Microsoft Graph when the user selects **View Related Content** for a row in the datagrid. The `search()` function calls `searchFiles()` in *graph.service.ts* and passes the `query` parameter to it (the name of the company in this example). The results of the search are then assigned to the `data` property of the component. 
    
    ```typescript
    override async search(query: string) {
        this.data = await this.graphService.searchFiles(query);
    }
    ```
    
    The files component can then use the `data` property to display the search results. You could handle this using custom HTML bindings or by using another Microsoft Graph Toolkit control named [`mgt-file-list`](https://learn.microsoft.com/en-us/graph/toolkit/components/file-list). Here's an example of binding the `data` property to one of the component's properties named `files` and handling the `itemClick` event as the user interacts with a file.

    ```html
    <mgt-file-list (itemClick)="itemClick($any($event))" [files]="data"></mgt-file-list>
    ```

1. Whether you choose to use the *mgt-search-results* component shown earlier or write custom code to call Microsoft Graph will depend on your scenario. In this example, the *mgt-search-results* component is used to simplify the code and reduce the amount of work you have to do.

### Exploring Teams Chat Messages Search Code

1. Go back to *graph.service.ts* and locate the `searchChatMessages()` function. You'll see that it's similar to the `searchFiles()` function you looked at previously.

    - It posts filter data to Microsoft Graph's `/search/query` API and converts the results into an array of objects that have information about the `teamId`, `channelId`, and `messageId` that match the search term.
    - To retrieve the Teams channel messages, a second call is made to the `/teams/${chat.teamId}/channels/${chat.channelId}/messages/${chat.messageId}` API and the `teamId`, `channelId`, and `messageId` are passed. This returns the full message details.
    - Additional filtering tasks are performed and the resulting messages are returned from `searchChatMessages()` to the caller.

1. Open the *chats.component.ts* file and locate the `search()` function. The full path to the file is *openai-acs-msgraph/client/src/app/chats/chats.component.ts*. The `search()` function calls `searchChatMessages()` in *graph.service.ts* and passes the `query` parameter to it. 

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

1. In addition to searching for Microsoft Teams chat messages, the application also allows a user to send messages to a Teams channel. This can be done by calling the `/teams/${teamId}/channels/${channelId}/messages` endpoint of Microsoft Graph. 

    :::image type="content" source="../media/viewing-teams-chat-dialog.png" alt-text="Sending a Teams Chat Message to a Channel":::

1. In the following code you'll see that a URL is created that includes the `teamId` and `channelId` values (environment variable values are used for the TeamsId and ChannelId in this example). The `body` constant contains the message to send. A POST request is then made and the results are returned to the caller.

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

1. Leveraging this type of functionality in Microsoft Graph provides a great way to enhance user productivbity by allowing users to interact with Microsoft Teams without leaving the application. 

### Exploring Email Messages Search Code

1. Open *emails.component.html* and take a moment to look through the code. The full path to the file is *openai-acs-msgraph/client/src/app/emails/emails.component.html*.

1. Locate the *mgt-search-results* component:

    ```html
    <mgt-search-results 
      class="search-results" 
      entity-types="message" 
      [queryString]="searchText"
      (dataChange)="dataChange($any($event))">
      <template data-type="result-message"></template>
    </mgt-search-results>
    ```

    This example of the *mgt-search-results* component is configured the same way as the one you looked at previously. The only difference is that the `entity-types` attribute is set to `message` which is used to search for email messages and an empty template is supplied.

        - The `class` attribute is used to specify that the `search-results` CSS class should be applied to the component.
        - The `entity-types` attribute is used to specify the type of data to search for. In this case, the value is `message`. 
        - The `queryString` attribute is used to specify the search term.
        - The `dataChange` event fires when the search results change. The emails component's `dataChange()` function is called, the results are passed to it, and a property named `data` is updated in the component. 
        - An empty template is defined for the component. This type of template is normally used to define how the search results will be rendered. However, in this scenario we're telling the component not to render any data. Instead, we'll render the data ourselves using standard data binding (Angular in this case but you could use any library/framework you want).

1. Look below the *mgt-search-results* component in *emails.component.html* to find the data bindings used to render the email messages. This example iterates through the `data` property and writes out the email subject, body preview, and a link to view the full email message.

    ```html
    <div *ngIf="data.length">
        <mat-card *ngFor="let email of data">
            <mat-card-header>
                <mat-card-title>{{email.resource.subject}}</mat-card-title>
                <mat-card-subtitle [innerHTML]="email.resource.bodyPreview"></mat-card-subtitle>
            </mat-card-header>
            <mat-card-actions>
                <a mat-stroked-button color="basic" [href]="email.resource.webLink" target="_blank">View Email Message</a>
            </mat-card-actions>
        </mat-card>
    </div>
    ```

    :::image type="content" source="../media/viewing-emails.png" alt-text="Viewing Email Messages":::
        
1. In addition to using the *mgt-search-results* component, Microsoft Graph provides several APIs that can be used to search email messages. The `/search/query` API that you saw earlier could certainly be used, but a more straightforward option is the `messages` API. To see how to call this API, go back to *graph.service.ts* and locate the `searchEmailMessages()` function. It creates a URL that can be used to call the `messages` endpoint of Microsoft Graph and embeds the `query` parameter in it. The code then makes a GET request and returns the results to the caller.

    ```typescript
    async searchEmailMessages(query:string) {
        if (!query) return [];
        // The $search operator will search the subject, body, and sender fields automatically
        const url = `https://graph.microsoft.com/v1.0/me/messages?$search="${query}"&$select=subject,bodyPreview,from,toRecipients,receivedDateTime,webLink`;
        const response = await Providers.globalProvider.graph.client.api(url).get();
        return response.value;
    }
    ```

1. The *emails* component located in *emails.component.ts* calls `searchEmailMessages()` and displays the results in the UI.

    ```typescript
    override async search(query: string) {
        this.data = await this.graphService.searchEmailMessages(query);
    }
    ```

### Exploring Calendar Events Search Code

1. Searching for calendar events can also be accomplished using the *mgt-search-results* component. It will handle rendering the results for you, but you can also define your own template as well which you'll see later in this exercise.

1. Open *calendar-events.component.html* and take a moment to look through the code. The full path to the file is *openai-acs-msgraph/client/src/app/calendar-events/calendar-events.component.html*. You'll see that it's very similar to the files and emails component you looked at previously.

    ```html
    <mgt-search-results 
        class="search-results" 
        entity-types="event" 
        [queryString]="searchText"
        (dataChange)="dataChange($any($event))">
        <template data-type="result-event"></template>
    </mgt-search-results>
    ```

    This example of the *mgt-search-results* component is configured the same way as the one you looked at previously with files and emails. The only difference is that the `entity-types` attribute is set to `event` which is used to search for calendar events and an empty template is supplied.

        - The `class` attribute is used to specify that the `search-results` CSS class should be applied to the component.
        - The `entity-types` attribute is used to specify the type of data to search for. In this case, the value is `event`. 
        - The `queryString` attribute is used to specify the search term.
        - The `dataChange` event fires when the search results change. The calendar *event* component's `dataChange()` function is called, the results are passed to it, and a property named `data` is updated in the component. 
        - An empty template is defined for the component. In this scenario we're telling the component not to render any data. Instead, we'll render the data ourselves using standard data binding (Angular in this case but you could use any library/framework you want).

1. Immediately below the `mgt-search-results` component in *calendar-events.component.html* you'll find the data bindings used to render the calendar events. This example iterates through the `data` property and writes out the start date, time, and subject of the event. Custom functions included in the component such as `dayFromDateTime()` and `timeRangeFromEvent()` are called to format data properly. The HTML bindings also include a link to view the event in Outlook and the location of the event if one is specified.

    ```html
    <div *ngIf="data.length">
        <div class="root" *ngFor='let event of data'>
            <div class="time-container">
                <div class="date">{{ dayFromDateTime(event.resource.start.dateTime)}}</div>
                <div class="time">{{ timeRangeFromEvent(event.resource) }}</div>
            </div>

            <div class="separator">
                <div class="vertical-line top"></div>
                <div class="circle">
                    <div *ngIf="!event.resource.bodyPreview?.includes('Join Microsoft Teams Meeting')"
                        class="inner-circle"></div>
                </div>
                <div class="vertical-line bottom"></div>
            </div>

            <div class="details">
                <div class="subject">{{ event.resource.subject }}</div>
                <div class="location" *ngIf="event.resource.location?.displayName">
                    at
                    <a href="https://bing.com/maps/default.aspx?where1={{event.resource.location.displayName}}"
                        target="_blank" rel="noopener"><b>{{ event.resource.location.displayName }}</b></a>
                </div>
                <div class="attendees" *ngIf="event.resource.attendees?.length">
                    <span class="attendee" *ngFor="let attendee of event.resource.attendees">
                        <mgt-person person-query="{{attendee.emailAddress.name}}"></mgt-person>
                    </span>
                </div>
                <div class="online-meeting"
                    *ngIf="event.resource.bodyPreview?.includes('Join Microsoft Teams Meeting')">
                    <img class="online-meeting-icon"
                        src="https://img.icons8.com/color/48/000000/microsoft-teams.png" title="Online Meeting" />
                    <a class="online-meeting-link" href="{{ event.resource.onlineMeetingUrl }}">Join Teams
                        Meeting</a>
                </div>
            </div>
        </div>
    </div>
    ```

    :::image type="content" source="../media/viewing-calendar-events.png" alt-text="Viewing Calendar Events":::

1. In addition to searching for calendar events using the `search/query` API, Microsoft Graph also provides an `events` API that can be used to search calendar events as well. Locate the `searchCalendarEvents()` function in *graph.service.ts*. It creates start and end date/time values that are used to define the time period to search. It then creates a URL that can be used to call the `events` endpoint of Microsoft Graph and includes the `query` parameter and start and end date/time variables. A GET request is then made and the results are returned to the caller.

    ```typescript
    async searchCalendarEvents(query:string) {
        if (!query) return [];
        const startDateTime = new Date();
        const endDateTime = new Date(startDateTime.getTime() + (7 * 24 * 60 * 60 * 1000));
        const url = `/me/events?startdatetime=${startDateTime.toISOString()}&enddatetime=${endDateTime.toISOString()}&$filter=contains(subject,'${query}')&orderby=start/dateTime`;

        const response = await Providers.globalProvider.graph.client.api(url).get();
        return response.value;
    }
    ```

    - Here's a breakdown of the URL that's created:

        - The `/me/events` portion of the URL is used to specify that the events of the signed in user should be retrieved.
        - The `startdatetime` and `enddatetime` parameters are used to define the time period to search. In this case, the search will return events that start within the next 7 days.
        - The `$filter` query parameter is used to filter the results by the `query` value (the company name selected from the datagrid in this case). The `contains()` function is used to look for the `query` value in the `subject` property of the calendar event.
        - The `$orderby` query paramter is used to order the results by the `start/dateTime` property.

    Once the `url` is created, a GET request is made to the Microsoft Graph API using the value of `url` and the results are returned to the caller.

1. As with the previous components, the *calendar-events* component (*calendar-events.component.ts* file) calls `search()` and displays the results.

    ```typescript
    override async search(query: string) {
        this.data = await this.graphService.searchCalendarEvents(query);
    }
    ```

    > [!NOTE]
    > You can make Microsoft Graph calls from a custom API or server-side application as well. View the [following tutorial](/microsoft-cloud/dev/tutorials/acs-to-teams-meeting) to see an example of calling a Microsoft Graph API from an Azure Function.