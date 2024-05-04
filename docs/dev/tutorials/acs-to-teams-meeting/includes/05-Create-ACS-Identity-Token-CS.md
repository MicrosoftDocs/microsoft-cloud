<!-- markdownlint-disable MD041 -->

1. Open *local.settings.json* and update the `ACS_CONNECTION_STRING` value with the ACS connection string you saved in an earlier exercise.

1. Open *Startup.cs* in Visual Studio and explore the second `AddSingleton()` call in the `ConfigureServices()` method.

    ```csharp
    var host = new HostBuilder()
        .ConfigureFunctionsWebApplication()
        .ConfigureServices(services => {

            ...

            services.AddSingleton(static p =>
            {
                var config = p.GetRequiredService<IConfiguration>();
                var connectionString = config.GetValue<string>("ACS_CONNECTION_STRING");
                return new CommunicationIdentityClient(connectionString);
            });

            ...

        })
        .Build();
    }
    ```

1. The `AddSingleton()` call creates a `CommunicationIdentityClient` object using the `ACS_CONNECTION_STRING` value from *local.settings.json*. 


1. Open *ACSTokenFunction.cs* and locate the constructor and field definitions. 

    - A field named `Scopes` is defined that includes the `CommunicationTokenScope.VoIP` scope. This scope is used to create the access token for the video call.

        ```csharp
        private static readonly CommunicationTokenScope[] Scopes =
        [
            CommunicationTokenScope.VoIP,
        ];
        ```

    - The `CommunicationIdentityClient` singleton instance created in *Startup.cs* is injected into the constructor and assigned to the `_tokenClient` field.

        ```csharp
        private readonly CommunicationIdentityClient _tokenClient;
        
        public ACSTokenFunction(CommunicationIdentityClient tokenClient)
        {
            _tokenClient = tokenClient;
        }
        ```

1. Explore the `Run()` method in *ACSTokenFunction.cs*:

    ```csharp
    [Function("HttpTriggerAcsToken")]
    public async Task<HttpResponseData> Run(
        [HttpTrigger(AuthorizationLevel.Anonymous, "get", Route = null)] HttpRequestData req,
        ILogger log)
    {
        var user = await _tokenClient.CreateUserAsync();
        var userToken = await _tokenClient.GetTokenAsync(user, Scopes);

        var response = req.CreateResponse(HttpStatusCode.OK);
        await response.WriteAsJsonAsync(
            new
            {
                userId = user.Value.Id,
                userToken.Value.Token,
                userToken.Value.ExpiresOn
            }
        );
        return response;
    }
    ```

    - It defines a function named of `HttpTriggerAcsToken` that can be called with an HTTP GET request.
    - A new ACS user is created by calling the `_tokenClient.CreateUserAsync()` method.
    - An access token used for video calls is created by calling the `_tokenClient. GetTokenAsync()` method.
    - The user ID and token are returned from the function as a JSON object.

1. Run the program by pressing <kbd>F5</kbd> in Visual Studio or by selecting **Debug --> Start Debugging** from the menu. This will start the Azure Functions project and make the `ACSTokenFunction` available to call.

    > [!NOTE]
    > If you're using VS Code you can open a terminal window in the *GraphACSFunctions* folder and run `func start`. This assumes that you have the [Azure Functions Core Tools](/azure/azure-functions/functions-run-local?tabs=linux%2Cisolated-process%2Cnode-v4%2Cpython-v2%2Chttp-trigger%2Ccontainer-apps&pivots=programming-language-csharp) installed on your machine.

1. Now that the Azure Functions are running locally, the client needs to be able to call into them to get the ACS user identity and token values.

1. Open *samples/acs-to-teams-meeting/client/react/App.tsx* file in your editor.

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

1. Open a terminal window and run `npm start` in the `react` folder. After it builds and loads you should see the ACS calling UI displayed and you can call into the Teams meeting that was dynamically created by Microsoft Graph.

1. Stop the React app by pressing <kbd>Ctrl + C</kbd> in the terminal window. 

1. Stop the Azure Functions project.

1. Commit your git changes and push them to your GitHub repository using Visual Studio Code:
    - Select the **Source Control** icon (3rd one down in the Visual Studio Code toolbar).
    - Enter a commit message and select **Commit**.
    - Select **Sync Changes**.