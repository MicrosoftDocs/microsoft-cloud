<!-- markdownlint-disable MD041 -->

Users must authenticate with Azure Active Directory (Azure AD) in order for Microsoft Graph to access organizational data. The Microsoft Graph Toolkit Login component can be used to authenticate users and retrieve an access token. The access token can then be used to make calls to Microsoft Graph.

In this exercise, you will:

- Learn how to associate an Azure AD app with the Microsoft Graph Toolkit so that it can be used to authenticate users and retrieve organizational data.
- Learn how the Microsoft Graph Toolkit's `mgt-login` component can be used to authenticate users and retrieve an access token.

### Exploring the Sign In Feature

1. In the [previous exercise](/microsoft-cloud/dev/tutorials/openai-msgraph-acs/?tutorial-step=7) you created an app registration in Azure AD and started the application server and API server. You also updated the following values in the `.env` file.

    ```
    AAD_CLIENT_ID=<APPLICATION_CLIENT_ID_VALUE>
    TEAM_ID=<TEAMS_TEAM_ID>
    CHANNEL_ID=<TEAMS_CHANNEL_ID>
    ```

1. Go back to the browser (*http://localhost:4200*), select **Sign In** in the header, and sign in using an account that is a member of your Microsoft 365 Developer tenant. 

    > [!NOTE]
    > You can view the users in your Microsoft 365 tenant by going to the [Microsoft 365 admin center](https://admin.microsoft.com/Adminportal/Home#/users).

1. Once you're signed in, you should see the name of the user displayed in the header.

    :::image type="content" source="../media/user-signed-in.png" alt-text="Signed in user":::

### Exploring the Sign In Code

> [!NOTE]
> If you're using Visual Studio Code, you can open files directly by selecting:
>
>   - Windows/Linux: <kbd>Ctrl + P</kdb>
>   - Mac: <kbd>Cmd + P</kdb>
>
> Then type the name of the file you want to open. 

1. Open *client/package.json* and notice that the `@microsoft/mgt` package is included in the dependencies. This package contains MSAL (Microsoft Authentication Library) provider features as well as web components such as `mgt-login` and others that can be used to sign in users and retrieve and display organizational data.

1. To use the `mgt-login` component to sign in users, the Azure AD app's client Id (stored in the *.env* file as `AAD_CLIENT_ID`) needs to be referenced and used.

1. Open *graph.service.ts* and locate the `init()` function. The full path to the file is *client/src/app/core/graph.service.ts*. You'll see the following code:

    ```typescript
    Providers.globalProvider = new Msal2Provider({
        clientId: AAD_CLIENT_ID, // retrieved from .env file
        scopes: ['User.Read', 'Chat.ReadWrite', 'Calendars.Read', 'ChannelMessage.Read.All', 'ChannelMessage.Send', 'Files.Read.All', 'Mail.Read',]
    });
    ```

    This code creates a new `Msal2Provider` object and passes the Azure AD client Id and the scopes that the app will request access to. The scopes are used to request access to the Microsoft Graph resources that the app will access. The `Msal2Provider` object is then assigned to the `Providers.globalProvider` object which is used by Microsoft Graph Toolkit components to retrieve data from Microsoft Graph.

1. Open *header.component.html* in your editor and locate the `mgt-login` component. The full path to the file is *client/src/app/header/header.component.html*.

    ```html
    <mgt-login *ngIf="featureFlags.microsoft365Enabled" class="mgt-dark" (loginCompleted)="loginCompleted()"></mgt-login>
    ```

    The `mgt-login` component is used to sign in users and retrieve an access token that can be used with Microsoft Graph. The `loginCompleted` event is emitted when the user has successfully signed in. The `mgt-login` component is only displayed if the `featureFlags.microsoft365Enabled` value is set to `true`.

1. Open *header.component.ts* and locate the `loginCompleted` function. This function is called when the `loginCompleted` event is emitted and used to retrieve the signed in user's profile using `Providers.globalProvider`. 

    ```typescript
    async loginCompleted() {
        const me = await Providers.globalProvider.graph.client.api('me').get();
        this.userLoggedIn.emit(me);
    }
    ```

    In this example, a call is being made to the `me` API in Microsoft Graph (`me` represents the current signed in user). The `userLoggedIn` event is emitted after the profile is retrieved to pass the data to the parent component so that it can access the user's display name.

1. Now that you've logged into the application, let's look at how organizational data can be retrieved.