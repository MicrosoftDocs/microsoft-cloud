---
title: Signing In a User and Getting an Access Token
description: Learn how to implement user authentication with Microsoft Entra ID and the Microsoft Graph Toolkit to obtain an access token for retrieving organizational data.
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

# Organizational Data: Signing In a User and Getting an Access Token

Users need to authenticate with Microsoft Entra ID  in order for Microsoft Graph to access organizational data. In this exercise, you'll see how the Microsoft Graph Toolkit's `mgt-login` component can be used to authenticate users and retrieve an access token. The access token can then be used to make calls to Microsoft Graph.

> [!NOTE]
> If you're new to Microsoft Graph, you can learn more about it in the [Microsoft Graph Fundamentals](/training/paths/m365-msgraph-fundamentals/?WT.mc_id=m365-94501-dwahlin) learning path. 

In this exercise, you will:

- Learn how to associate a Microsoft Entra ID app with the Microsoft Graph Toolkit to authenticate users and retrieve organizational data.
- Learn about the importance of scopes.
- Learn how the Microsoft Graph Toolkit's *mgt-login* component can be used to authenticate users and retrieve an access token.

### Using the Sign In Feature

1. In the [previous exercise](/microsoft-cloud/dev/tutorials/openai-acs-msgraph/09-orgdata-create-entraid-app.md), you created an app registration in Microsoft Entra ID and started the application server and API server. You also updated the following values in the `.env` file (`TEAM_ID` and `CHANNEL_ID` are optional):

    ```
    ENTRAID_CLIENT_ID=<APPLICATION_CLIENT_ID_VALUE>
    TEAM_ID=<TEAMS_TEAM_ID>
    CHANNEL_ID=<TEAMS_CHANNEL_ID>
    ```

    Ensure you've completed the [previous exercise](/microsoft-cloud/dev/tutorials/openai-acs-msgraph/09-orgdata-create-entraid-app.md) before continuing.

1. Go back to the browser (*http://localhost:4200*), select **Sign In** in the header, and sign in using an admin user account from your Microsoft 365 Developer tenant.

    > [!TIP]
    > Sign in with your Microsoft 365 developer tenant admin account. You can view other users in your developer tenant by going to the [Microsoft 365 admin center](https://admin.microsoft.com/Adminportal/Home#/users).

1. If you're signing in to the application for the first time, you'll be prompted to consent to the permissions requested by the application. You'll learn more about these permissions (also called "scopes") in the next section as you explore the code. Select **Accept** to continue.

1. Once you're signed in, you should see the name of the user displayed in the header.

    :::image type="content" source="./media/user-signed-in.png" alt-text="Signed in user":::

### Exploring the Sign In Code

Now that you've signed in, let's look at the code used to sign in the user and retrieve an access token and user profile. You'll learn about the *mgt-login* web component that's part of the Microsoft Graph Toolkit.

[!INCLUDE [Note-Open-Files-VS-Code](./includes/tip-open-files-vs-code.md)]

1. Open *client/package.json* and notice that the `@microsoft/mgt` and `@microsoft/mgt-components` packages are included in the dependencies. The `@microsoft/mgt` package contains MSAL (Microsoft Authentication Library) provider features and web components such as *mgt-login* and others that can be used to sign in users and retrieve and display organizational data.

1. Open *client/src/main.ts* and notice the following imports from the `@microsoft/mgt-components` package. The imported symbols are used to register the Microsoft Graph Toolkit components that are used in the application.

    ```typescript
    import { registerMgtLoginComponent, registerMgtSearchResultsComponent, registerMgtPersonComponent,  } from '@microsoft/mgt-components';
    ```

1. Scroll to the bottom of the file and note the following code:

    ```typescript
    registerMgtLoginComponent();
    registerMgtSearchResultsComponent();
    registerMgtPersonComponent();
    ```

    This code registers the `mgt-login`, `mgt-search-results`, and `mgt-person` web components and enables them for use in the application. 

1. To use the *mgt-login* component to sign in users, the Microsoft Entra ID app's client Id (stored in the *.env* file as `ENTRAID_CLIENT_ID`) needs to be referenced and used.

1. Open *graph.service.ts* and locate the `init()` function. The full path to the file is *client/src/app/core/graph.service.ts*. You'll see the following import and code:

    ```typescript
    import { Msal2Provider, Providers, ProviderState } from '@microsoft/mgt';

    init() {
        if (!this.featureFlags.microsoft365Enabled) return;

        if (!Providers.globalProvider) {
            console.log('Initializing Microsoft Graph global provider...');
            Providers.globalProvider = new Msal2Provider({
                clientId: ENTRAID_CLIENT_ID,
                scopes: ['User.Read', 'Presence.Read', 'Chat.ReadWrite', 'Calendars.Read', 
                        'ChannelMessage.Read.All', 'ChannelMessage.Send', 'Files.Read.All', 'Mail.Read']
            });
        }
        else {
            console.log('Global provider already initialized');
        }
    }
    ```

    This code creates a new `Msal2Provider` object, passing the Microsoft Entra ID client Id from your app registration and the `scopes` for which the app will request access. The `scopes` are used to request access to Microsoft Graph resources that the app will access. After the `Msal2Provider` object is created, it's assigned to the `Providers.globalProvider` object, which is used by Microsoft Graph Toolkit components to retrieve data from Microsoft Graph.

1. Open *header.component.html* in your editor and locate the *mgt-login* component. The full path to the file is *client/src/app/header/header.component.html*.

    ```html
    @if (this.featureFlags.microsoft365Enabled) {
        <mgt-login class="mgt-dark" (loginCompleted)="loginCompleted()"></mgt-login>
    }
    ```

    The *mgt-login* component enables user sign in and provides access to a token that is used with Microsoft Graph. Upon successful sign in, the `loginCompleted` event is triggered, which calls the `loginCompleted()` function. Although *mgt-login* is used within an Angular component in this example, it is compatible with any web application.

    Display of the *mgt-login* component depends on the `featureFlags.microsoft365Enabled` value being set to `true`. This custom flag checks for the presence of the `ENTRAID_CLIENT_ID` environment variable to confirm that the application is properly configured and able to authenticate against Microsoft Entra ID. The flag is added to accommodate cases where users choose to complete only the AI or Communication exercises within the tutorial, rather than following the entire sequence of exercises.
    
1. Open *header.component.ts* and locate the `loginCompleted` function. This function is called when the `loginCompleted` event is emitted and handles retrieving the signed in user's profile using `Providers.globalProvider`.

    ```typescript
    async loginCompleted() {
        const me = await Providers.globalProvider.graph.client.api('me').get();
        this.userLoggedIn.emit(me);
    }
    ```

    In this example, a call is being made to the Microsoft Graph `me` API to retrieve the user's profile (`me` represents the current signed in user). The `this.userLoggedIn.emit(me)` code statement emits an event from the component to pass the profile data to the parent component. The parent component is *app.component.ts* file in this case, which is the root component for the application.

    To learn more about the *mgt-login* component, visit the [Microsoft Graph Toolkit](/graph/toolkit/components/login?WT.mc_id=m365-94501-dwahlin) documentation.

1. Now that you've logged into the application, let's look at how organizational data can be retrieved.

## Next Step

> [!div class="nextstepaction"]
> [Organizational Data: Retrieving Files, Chats, and Sending Messages to Teams](./11-orgdata-retrieving-files-chats.md)