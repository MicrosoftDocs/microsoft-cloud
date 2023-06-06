<!-- markdownlint-disable MD041 -->

1. Open the `samples/acs-to-teams-meeting/server/csharp/GraphACSFunctions.sln` in Visual Studio or open the *GraphACSFunctions* folder in Visual Studio Code.

1. Go to the `GraphACSFunctions` project and create a `local.settings.json` file with the following values:

    ```json
    {
        "IsEncrypted": false,
        "Values": {
            "FUNCTIONS_WORKER_RUNTIME": "node",
            "TENANT_ID": "",
            "CLIENT_ID": "",
            "CLIENT_SECRET": "",
            "USER_ID": "",
            "ACS_CONNECTION_STRING": ""
        },
        "Host": {
            "LocalHttpPort": 7071,
            "CORS": "*",
            "CORSCredentials": false
        },
        "watchDirectories": [
            "Shared"
        ]
    }
    ```

    - Use the values you copied into the local file in an earlier exercise to update the `TENANT_ID`, `CLIENT_ID` and `CLIENT_SECRET` values.
    - Define `USER_ID` with the user id that you'd like to create a Microsoft Teams Meeting. 

    You can get the User ID from the [Azure Portal](https://portal.azure.com).

        - Login using your Microsoft 365 developer tenant admin account.
        - Select **Azure Active Directory**,
        - Navigate to the **Users** tab on the side bar. 
        - Search for your user name and select it to see the user details. 
        - Inside the user details, `Object ID` represents the `User ID`. Copy the `Object ID` value and use it for the `USER_ID` value in *local.settings.json*.

    :::image type="content" source="../media/aad-user-id.png" alt-text="Getting User ID from Azure Active Directory":::

    > [!NOTE]
    > `ACS_CONNECTION_STRING` will be used in the next exercise so you don't need to update it yet.

1. Open `GraphACSFunctions.sln` located in the *acs-to-teams-meeting/server/csharp* folder and note that it includes the following Microsoft Graph and Identity packages:

    ```xml
    <PackageReference Include="Azure.Communication.Identity" Version="1.2.0" />
    <PackageReference Include="Azure.Identity" Version="1.8.2" />
    <PackageReference Include="Microsoft.Graph" Version="4.53.0" />
    ```

1. Go to *Startup.cs* and note the following code in the `Configure` method:

    ```csharp
    public override void Configure(IFunctionsHostBuilder builder)
    {
        builder.Services.AddSingleton(static p =>
        {
            var config = p.GetRequiredService<IConfiguration>();
            var clientSecretCredential = new ClientSecretCredential(
                config.GetValue<string>("TENANT_ID"),
                config.GetValue<string>("CLIENT_ID"),
                config.GetValue<string>("CLIENT_SECRET")
            );

            return new GraphServiceClient(
                clientSecretCredential,
                new[] { "https://graph.microsoft.com/.default" }
            );
        });

        ...

        builder.Services.AddSingleton<IGraphService, GraphService>();
    }
    ```

    - This code creates a `GraphServiceClient` object that can be used to call Microsoft Graph from Azure Functions. It's created as a singleton and can be injected into other classes.
    - You can make Microsoft Graph API calls with [app-only permissions](graph/auth/auth-concepts#access-scenarios) (such as **Calendars.ReadWrite**) by passing a `ClientSecretCredential` value to the `GraphServiceClient` constructor. The `ClientSecretCredential` uses the `Tenant Id`, `Client Id` and `Client Secret` values from the Azure Active Directory app.
    
1. Open *Services/GraphService.cs*. 

1. Take a moment to explore the `CreateMeetingEventAsync` method:

    ```csharp
    using System;
    using System.Threading.Tasks;
    using Microsoft.Graph;
    using Microsoft.Extensions.Configuration;
    
    namespace GraphACSFunctions.Services;
    
    public class GraphService : IGraphService
    {
        private readonly GraphServiceClient _graphServiceClient;
        private readonly IConfiguration _configuration;
    
        public GraphService(GraphServiceClient graphServiceClient, IConfiguration configuration)
        {
            _graphServiceClient = graphServiceClient;
            _configuration = configuration;
        }
    
        public async Task<string> CreateMeetingAsync()
        {
            var userId = _configuration.GetValue<string>("USER_ID");
            var newMeeting = await _graphServiceClient
                .Users[userId]
                .Calendar
                .Events
                .Request()
                .AddAsync(new()
                {
                    Subject = "Customer Service Meeting",
                    Start = new()
                    {
                        DateTime = DateTime.UtcNow.ToString("yyyy-MM-ddTHH:mm:ss"),
                        TimeZone = "UTC"
                    },
                    End = new()
                    {
                        DateTime = DateTime.UtcNow.AddHours(1).ToString("yyyy-MM-ddTHH:mm:ss"),
                        TimeZone = "UTC"
                    },
                    IsOnlineMeeting = true
                });
            return newMeeting.OnlineMeeting.JoinUrl;
        }
    }
    ```

    - `GraphServiceClient` and `IConfiguration` objects are injected into the constructor and assigned to fields.
    - The `CreateMeetingAsync()` function posts data to the [Microsoft Graph Calendar Events API](/graph/api/calendar-post-events?view=graph-rest-1.0&tabs=http) which dynamically creates an event in a user's calendar and returns the join URL.


1. Open *TeamsMeetingFunctions.cs* and take a moment to examine its constructor.The `GraphServiceClient` you looked at earlier is injected and assigned to the `_graphService` field.

    ```csharp
    private readonly IGraphService _graphService;
    
    public TeamsMeetingFunction(IGraphService graphService) => _graphService = graphService;
    ```

1. Locate the `Run` method:

    ```csharp
    [FunctionName("TeamsMeetingFunction")]
    public async Task<IActionResult> Run(
        [HttpTrigger(AuthorizationLevel.Anonymous, "get", Route = null)] HttpRequest req,
        ILogger log) => 
        new OkObjectResult(await _graphService.CreateMeetingAsync());
    ```

    - It defines a function named of `TeamsMeetingFunction` that can be called with an HTTP GET request.
    - It calls `_graphService.CreateMeetingAsync()` which creates a new event and returns the join URL.
    
1. Run the program by pressing <kbd>F5</kbd> in Visual Studio or by selecting **Debug --> Start Debugging** from the menu. This will start the Azure Functions project and make the `ACSTokenFunction` available to call.

    > [!NOTE]
    > If you're using VS Code you can open a terminal window in the *GraphACSFunctions* folder and run `dotnet run`.

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
