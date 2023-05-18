<!-- markdownlint-disable MD041 -->

Users need to authenticate with Azure Active Directory (Azure AD) in order for Microsoft Graph to access organizational data. In this exercise you'll see how the Microsoft Graph Toolkit's `mgt-login` component can be used to authenticate users and retrieve an access token. The access token can then be used to make calls to Microsoft Graph.

> [!NOTE]
> If you're new to Microsoft Graph, you can learn more about it in the [Microsoft Graph Fundamentals](/training/paths/m365-msgraph-fundamentals/?WT.mc_id=m365-94501-dwahlin) learning path. 

In this exercise, you will:

- Learn how to associate an Azure AD app with the Microsoft Graph Toolkit so that it can be used to authenticate users and retrieve organizational data.
- Learn about the importance of scopes.
- Learn how the Microsoft Graph Toolkit's *mgt-login* component can be used to authenticate users and retrieve an access token.

### Using the Sign In Feature

1. In the [previous exercise](/microsoft-cloud/dev/tutorials/openai-acs-msgraph/?tutorial-step=8) you created an app registration in Azure AD and started the application server and API server. You also updated the following values in the `.env` file.

    ```
    AAD_CLIENT_ID=<APPLICATION_CLIENT_ID_VALUE>
    TEAM_ID=<TEAMS_TEAM_ID>
    CHANNEL_ID=<TEAMS_CHANNEL_ID>
    ```

    Ensure you've completed the [previous exercise](/microsoft-cloud/dev/tutorials/openai-acs-msgraph/?tutorial-step=8) before continuing.

1. Go back to the browser (*http://localhost:4200*), select **Sign In** in the header, and sign in with a user from your Microsoft 365 Developer tenant.

    > [!TIP]
    > You can sign in with your Microsoft 365 developer tenant admin account. You can view other users in your developer tenant by going to the [Microsoft 365 admin center](https://admin.microsoft.com/Adminportal/Home#/users).

1. If you're signing in to the application for the first time, you'll be prompted to consent to the permissions requested by the application. You'll learn more about these permissions (also called "scopes") in the next section as you explore the code. Select **Accept** to continue.

1. Once you're signed in, you should see the name of the user displayed in the header.

    :::image type="content" source="../media/user-signed-in.png" alt-text="Signed in user":::

### Exploring the Sign In Code

Now that you've signed in, let's look at the code used to sign in the user and retrieve an access token and user profile. You'll learn about the *mgt-login* web component that's part of the Microsoft Graph Toolkit.

[!INCLUDE [Note-Open-Files-VS-Code](./tip-open-files-vs-code.md)]

1. Open *client/package.json* and notice that the `@microsoft/mgt` package is included in the dependencies. This package contains MSAL (Microsoft Authentication Library) provider features as well as web components such as *mgt-login* and others that can be used to sign in users and retrieve and display organizational data.

1. To use the *mgt-login* component to sign in users, the Azure AD app's client Id (stored in the *.env* file as `AAD_CLIENT_ID`) needs to be referenced and used.

1. Open *graph.service.ts* and locate the `init()` function. The full path to the file is *client/src/app/core/graph.service.ts*. You'll see the following code:

    ```typescript
    Providers.globalProvider = new Msal2Provider({
        clientId: AAD_CLIENT_ID, // retrieved from .env file
        scopes: ['User.Read', 'Presence.Read', 'Chat.ReadWrite', 'Calendars.Read', 
                 'ChannelMessage.Read.All', 'ChannelMessage.Send', 'Files.Read.All', 'Mail.Read']
    });
    ```

    This code creates a new `Msal2Provider` object, passing the Azure AD client Id from your app registration and the `scopes` for which the app will request access. The `scopes` are used to request access to Microsoft Graph resources that the app will access. After the `Msal2Provider` object is created, it's assigned to the `Providers.globalProvider` object which is used by Microsoft Graph Toolkit components to retrieve data from Microsoft Graph.

1. Open *header.component.html* in your editor and locate the *mgt-login* component. The full path to the file is *client/src/app/header/header.component.html*.

    ```html
    <mgt-login *ngIf="featureFlags.microsoft365Enabled" class="mgt-dark" (loginCompleted)="loginCompleted()"></mgt-login>
    ```

    The *mgt-login* component enables user sign in and access token retrieval for use with Microsoft Graph. Upon successful sign in, the `loginCompleted` event is triggered, subsequently calling the `loginCompleted()` function. Although *mgt-login* is used within an Angular component in this example, it is compatible with any web application.

    Display of the *mgt-login* component depends on the `featureFlags.microsoft365Enabled` value being set to `true`. This custom flag checks for the presence of the `AAD_CLIENT_ID` environment variable to confirm that the application is properly configured and able to authenticate against Azure AD. The flag is added to accommodate cases where users opt to complete only the AI or Communication exercises within the tutorial, rather than following the entire sequence.
    
1. Open *header.component.ts* and locate the `loginCompleted` function. This function is called when the `loginCompleted` event is emitted and used to retrieve the signed in user's profile using `Providers.globalProvider`. 

    ```typescript
    async loginCompleted() {
        const me = await Providers.globalProvider.graph.client.api('me').get();
        this.userLoggedIn.emit(me);
    }
    ```

    In this example, a call is being made to the Microsoft Graph `me` API to retrieve their user profile (`me` represents the current signed in user). The `this.userLoggedIn.emit(me)` code statement emits an event from the component to pass the profile data to the parent component. The parent component is *app.component.ts* file in this case, which is the root component for the application.

    To learn more about the *mgt-login* component visit the [Microsoft Graph Toolkit](/graph/toolkit/components/login?WT.mc_id=m365-94501-dwahlin) documentation.

1. Now that you've logged into the application, let's look at how organizational data can be retrieved.