<!-- markdownlint-disable MD041 -->

1. Open the `samples/acs-to-teams-meeting/server/csharp/GraphACSFunctions.sln` in Visual Studio or open the *GraphACSFunctions* folder in Visual Studio Code.

1. Go to the `GraphACSFunctions` project and create a `local.settings.json` file with the following values:

    ```json
    {
        "IsEncrypted": false,
        "Values": {
            "FUNCTIONS_WORKER_RUNTIME": "dotnet-isolated",
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

    You can get the User ID from the [Azure portal](https://portal.azure.com).

    - Login using your Microsoft 365 developer tenant admin account.
    - Select **Microsoft Entra ID**
    - Navigate to the **Users** tab on the side bar. 
    - Search for your user name and select it to see the user details. 
    - Inside the user details, `Object ID` represents the `User ID`. Copy the `Object ID` value and use it for the `USER_ID` value in *local.settings.json*.

    :::image type="content" source="./media/aad-user-id.png" alt-text="Getting User ID from Microsoft Entra ID":::

    > [!NOTE]
    > `ACS_CONNECTION_STRING` will be used in the next exercise so you don't need to update it yet.

1. Open `GraphACSFunctions.sln` located in the *acs-to-teams-meeting/server/csharp* folder and note that it includes the following Microsoft Graph and Identity packages:

    ```xml
    <PackageReference Include="Azure.Communication.Identity" Version="1.3.1" />
    <PackageReference Include="Azure.Identity" Version="1.11.2" />
    <PackageReference Include="Microsoft.Graph" Version="5.51.0" />
    ```

1. Go to *Program.cs* and note the following code in the `ConfigureServices` method:

    ```csharp
        var host = new HostBuilder()
            .ConfigureFunctionsWebApplication()
            .ConfigureServices(services => {
                services.AddApplicationInsightsTelemetryWorkerService();
                services.ConfigureFunctionsApplicationInsights();
                services.AddSingleton(static p =>
                {
                    var config = p.GetRequiredService<IConfiguration>();
                    var clientSecretCredential = new ClientSecretCredential(
                        config.GetValue<string>("TENANT_ID"),
                        config.GetValue<string>("CLIENT_ID"),
                        config.GetValue<string>("CLIENT_SECRET")
                    );

                    return new GraphServiceClient(
                        clientSecretCredential,
                        ["https://graph.microsoft.com/.default"]
                    );
                });

                ...

                services.AddSingleton<IGraphService, GraphService>();
            })
            .Build();
    }
    ```

    - This code creates a `GraphServiceClient` object that can be used to call Microsoft Graph from Azure Functions. It's a singleton and can be injected into other classes.
    - You can make Microsoft Graph API calls with [app-only permissions](/graph/auth/auth-concepts#access-scenarios) (such as **Calendars.ReadWrite**) by passing a `ClientSecretCredential` value to the `GraphServiceClient` constructor. The `ClientSecretCredential` uses the `Tenant Id`, `Client Id` and `Client Secret` values from the Microsoft Entra ID app.
    
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
                .PostAsync(new()
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
    - The `CreateMeetingAsync()` function posts data to the [Microsoft Graph Calendar Events API](/graph/api/calendar-post-events?view=graph-rest-1.0&tabs=http), which dynamically creates an event in a user's calendar and returns the join URL.


1. Open *TeamsMeetingFunctions.cs* and take a moment to examine it's constructor. The `GraphServiceClient` you looked at earlier is injected and assigned to the `_graphService` field.

    ```csharp
    private readonly IGraphService _graphService;
    
    public TeamsMeetingFunction(IGraphService graphService) => _graphService = graphService;
    ```

1. Locate the `Run` method:

    ```csharp
    [Function("HttpTriggerTeamsUrl")]
    public async Task<HttpResponseData> Run(
        [HttpTrigger(AuthorizationLevel.Anonymous, "get", Route = null)] HttpRequestData req,
        ILogger log)
    {
        var response = req.CreateResponse(HttpStatusCode.OK);
        await response.WriteStringAsync(await _graphService.CreateMeetingAsync());
        return response;
    }
    ```

    - It defines a function name of `HttpTriggerTeamsUrl` that can be called with an HTTP GET request.
    - It calls `_graphService.CreateMeetingAsync()`, which creates a new event and returns the join URL.
    
1. Run the program by pressing <kbd>F5</kbd> in Visual Studio or by selecting **Debug --> Start Debugging** from the menu. This action starts the Azure Functions project and make the `ACSTokenFunction` available to call.

> [!NOTE]
> If you're using VS Code you can open a terminal window in the *GraphACSFunctions* folder and run `func start`. This assumes that you have the [Azure Functions Core Tools](/azure/azure-functions/functions-run-local?tabs=linux%2Cisolated-process%2Cnode-v4%2Cpython-v2%2Chttp-trigger%2Ccontainer-apps&pivots=programming-language-csharp) installed on your machine.
