<!-- markdownlint-disable MD041 -->

1. Open *local.settings.json* and update the `ACS_CONNECTION_STRING` value with the ACS connection string you saved in an earlier exercise.

1. Open *server/typescript/src/functions/httpTriggerAcsToken.ts* in Visual Studio Code. It has the following code:

    ```typescript
    import { app, HttpRequest, HttpResponseInit, InvocationContext } from "@azure/functions";
    import { CommunicationIdentityClient } from '@azure/communication-identity';

    export async function httpTriggerAcsToken(request: HttpRequest, context: InvocationContext): Promise<HttpResponseInit> {
        // Get ACS connection string from local.settings.json (or App Settings when in Azure)
        const connectionString = process.env.ACS_CONNECTION_STRING;
        let tokenClient = new CommunicationIdentityClient(connectionString);
        const user = await tokenClient.createUser();
        const userToken = await tokenClient.getToken(user, ["voip"]);
        return {
            jsonBody: { userId: user.communicationUserId, ...userToken }
        };
    }

    app.http('httpTriggerAcsToken', {
        methods: ['GET'],
        authLevel: 'anonymous',
        handler: httpTriggerAcsToken
    });

    export default httpTriggerAcsToken;
    ```

1. The function performs the following tasks:
    - Imports `CommunicationIdentityClient` which will be used to create the user identity and token.

        ```typescript
        import { CommunicationIdentityClient } from '@azure/communication-identity';
        ```

    - Gets the ACS connection string from an environment variable named `ACS_CONNECTION_STRING`.

        ```typescript
        const connectionString = process.env.ACS_CONNECTION_STRING;
        ```

    > [!NOTE]
    > This is the connection string value you added into the *local.settings.json* file earlier.

    - Creates a new `CommunicationIdentityClient` instance and passes the ACS connection string to it.

        ```typescript
        let tokenClient = new CommunicationIdentityClient(connectionString);
        ```

    - Creates an ACS user and gets a Voice Over IP token.

        ```typescript
        const user = await tokenClient.createUser();
        const userToken = await tokenClient.getToken(user, ["voip"]);
        ```

    - Sends the userId and token values back to the caller.

        ```typescript
        return {
            jsonBody: { userId: user.communicationUserId, ...userToken }
        };
        ```

1. Go to the *server/typescript* folder in a terminal window and run `npm start`.

1. Now that the Azure Functions are running locally, the client needs to be able to call into them to get the ACS user identity and token values.

1. Open *client/react/App.tsx* file in your editor.

1. Locate the `userId` and `token` state variables in the component. Remove the hardcoded values and replace them with empty quotes:

    ```typescript
    const [userId, setUserId] = useState<string>('');
    const [token, setToken] = useState<string>('');
    ```

1. Locate the `useEffect` function and change it to look like the following to enable calling the Azure Function to retrieve an ACS user identity and token:

    ```typescript
    useEffect(() => {
        const init = async () => {
            setMessage('Getting ACS user');
            //Call Azure Function to get the ACS user identity and token
            let res = await fetch(process.env.REACT_APP_ACS_USER_FUNCTION as string);
            let user = await res.json();
            setUserId(user.userId);
            setToken(user.token);
            
            setMessage('Getting Teams meeting link...');
            //Call Azure Function to get the meeting link
            res = await fetch(process.env.REACT_APP_TEAMS_MEETING_FUNCTION as string);
            let link = await res.text();
            setTeamsMeetingLink(link);
            setMessage('');
            console.log('Teams meeting link', link);
        }
        init();

    }, []);
    ```

1. Save the file before continuing.

1. Open a separate terminal and run `npm start` in the *react* folder. After it builds and loads you should see the ACS calling UI displayed and you can call into the Teams meeting that was dynamically created by Microsoft Graph.

1. Stop both of the terminal processes (React and Azure Functions) by selecting <kbd>Ctrl + C</kbd>.

1. Commit your git changes and push them to your GitHub repository using Visual Studio Code:
    - Select the **Source Control** icon (3rd one down in the Visual Studio Code toolbar).
    - Enter a commit message and select **Commit**.
    - Select **Sync Changes**.