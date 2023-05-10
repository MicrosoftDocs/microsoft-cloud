<!-- markdownlint-disable MD041 -->

In the [previous exercise](/microsoft-cloud/dev/tutorials/openai-acs-msgraph/?tutorial-step=9) you learned how to retrieve files from OneDrive for Business and chats from Microsoft Teams using Microsoft Graph and the *mgt-search-results* component from Microsoft Graph Toolkit. You also learned how to send messages to Microsoft Teams Channels. In this exercise, you will learn how to retrieve email messages and calendar events from Microsoft Graph and integrate them into the application.

In this exercise, you will:

- Learn how the *mgt-search-results* web component in the Microsoft Graph Toolkit can be used to search for emails and calendar events.
- Learn how to customize the *mgt-search-results* component to render search results in a custom way.
- Learn how to call Microsoft Graph directly to retrieve emails and calendar events.

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