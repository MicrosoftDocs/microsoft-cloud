<!-- markdownlint-disable MD041 -->

In this exercise, you'll automate the process of creating a Microsoft Teams meeting link and passing to the ACS by using Azure Functions and Microsoft Graph.

:::image type="content" source="../media/3-create-teams-meeting-link.png" alt-text="Create Teams Meeting":::

1. You'll need to create an Azure Active Directory (AAD) app for Daemon app authentication. In this step, authentication will be handled in the background with *app credentials*, and AAD app will use Application Permissions to make Microsoft Graph API calls. Microsoft Graph will be used to dynamically create a Microsoft Teams meeting and return the Teams meeting URL.

1. Perform the following steps to create an AAD app:
    1. Go to [Azure Portal](https://portal.azure.com) and select **Azure Active Directory**.
    1. Select the **App registration** tab followed by **+ New registration**.
    1. Fill in the new app registration form details as shown below and select **Register**:
        - Name: *ACS Teams Interop App*
        - Supported account types: *Accounts in any organizational directory (Any Azure AD directory - Multitenant) and personal Microsoft accounts (e.g. Skype, Xbox)*
        - Redirect URI: leave this blank
    1. After the app is registered, go to **API permissions** and select **+ Add a permission**.
    1. Select **Microsoft Graph** followed by **Application permissions**.
    1. Select the `Calendars.ReadWrite` permission and then select **Add**.
    1. After adding the permissions, select **Grant admin consent for <YOUR_ORGANIZATION_NAME>**.
    1. Go to the **Certificates & secrets** tab, select **+ New client secret**, and then select **Add**. 
    1. Copy the value of the secret into a local file. You'll use the value later in this exercise.
    1. Go to the **Overview** tab and copy the `Application (client) ID` and `Directory (tenant) ID` values into the same local file that you used in the previous step.

# [C#](#tab/csharp)

[!INCLUDE [CSharp](./04-Create-Teams-Meeting-CS.md)]

# [TypeScript](#tab/typescript)

[!INCLUDE [TypeScript](./04-Create-Teams-Meeting-TS.md)]

---

## Calling the Azure Function from React

1. Now that the `TeamsMeetingFunction` is ready to use, let's call the function from the React app.

1. Open the *samples/acs-to-teams-meeting/client/react* folder in VS Code. 

1. Add an *.env* file into the folder with the following values:

    ```
    REACT_APP_TEAMS_MEETING_FUNCTION=http://localhost:7071/api/TeamsMeetingFunction

    REACT_APP_ACS_USER_FUNCTION=http://localhost:7071/api/ACSTokenFunction
    ```

    These values will be passed into React as it builds so that you can easily change them as needed during the build process.

1. Open *samples/acs-to-teams-meeting/client/react/App.tsx* file in VS Code.

1. Locate the `teamsMeetingLink` state variable in the component. Remove the hardcoded teams link and replace it with empty quotes:

    ```typescript
    const [teamsMeetingLink, setTeamsMeetingLink] = useState<string>('');
    ```

1. Locate the `useEffect` function and change it to look like the following. This handles calling the Azure Function you looked at earlier which creates a Teams meeting and returns the meeting join link:

    ```typescript
    useEffect(() => {
        const init = async () => {
            /* Commenting out for now
            setMessage('Getting ACS user');
            //Call Azure Function to get the ACS user identity and token
            const res = await fetch(process.env.REACT_APP_ACS_USER_FUNCTION as string);
            const user = await res.json();
            setUserId(user.userId);
            setToken(user.token);
            */
            
            setMessage('Getting Teams meeting link...');
            //Call Azure Function to get the meeting link
            const resTeams = await fetch(process.env.REACT_APP_TEAMS_MEETING_FUNCTION as string);
            const link = await resTeams.text();
            setTeamsMeetingLink(link);
            setMessage('');
            console.log('Teams meeting link', link);

        }
        init();

    }, []);
    ```

1. Save the file before continuing.

1. Open a terminal window, `cd` into the *react folder, and run `npm start` to build and run the application.

1. After the application builds and loads, you should see the ACS calling UI displayed and can then call into the Teams meeting that was dynamically created by Microsoft Graph.

1. Stop the terminal process by entering <kbd>Ctrl + C</kbd> in the terminal window.

1. Stop the Azure Functions project.

> [!NOTE]
> Visit the Azure Communication Services documentation to learn more about [extending Microsoft Teams meetings](/azure/communication-services/tutorials/virtual-visits/extend-teams/overview) in other ways.




