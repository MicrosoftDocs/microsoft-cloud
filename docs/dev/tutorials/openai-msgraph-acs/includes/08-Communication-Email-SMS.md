<!-- markdownlint-disable MD041 -->

In addition to phone calls, Azure Communication Services can also be used to send email and SMS messages. This can be useful when you want to send a message to a customer or other user directly from the application.

In this exercise, you will:
- Explore how email and SMS messages can be sent from the application.
- Walk through the code to learn how the email and SMS functionality is implemented.

### Exploring the Email and SMS Features

1. In a [previous exercise](/microsoft-cloud/dev/tutorials/openai-msgraph-acs/?tutorial-step=5) you created an Azure Communication Services (ACS) resource and started the database, web server, and API server. This included adding an ACS connection string, ACS email address, customer email address, and customer phone number into the project's `.env` file.

    ```
    ACS_CONNECTION_STRING=<ACS_CONNECTION_STRING>
    ACS_PHONE_NUMBER=<ACS_PHONE_NUMBER>
    ACS_EMAIL_ADDRESS=<ACS_EMAIL_ADDRESS>
    CUSTOMER_EMAIL_ADDRESS=<EMAIL_ADDRESS_TO_SEND_EMAIL_TO>
    CUSTOMER_PHONE_NUMBER=<UNITED_STATES_BASED_NUMBER_TO_SEND_SMS_TO>
    ```

1. Go back to the browser (*http://localhost:4200*), locate the datagrid, and select **Contact Customer** followed by **Email/SMS Customer** in the first row.

    :::image type="content" source="../media/acs-email-sms-customer.png" alt-text="Send an email or SMS message using ACS.":::

1. In the Email/SMS tab, perform the following tasks:

    - Enter an Email **Subject** and **Body** and select the **Send Email** button.
    - Enter an SMS message and select the **Send SMS** button.

1. Check the email address and phone number you defined in the *.env* file to view the messages. 

    > [!NOTE]
    > If you don't see the email message in your inbox for the address you defined in the `.env` file, check your spam folder.

### Exploring the Email Code

1. Select <kbd>Ctrl + P</kdb> (Windows/Linux) or <kbd>Cmd + P</kdb> (Mac) based on your machine, and select the `customers-list.component.ts` file. The full path to the file is `openai-msgraph-acs/client/src/app/customers-list/customers-list.component.ts`.

1. Note that `openCallDialog()` sends a `CustomerCall` message and the customer phone number using an event bus.
    
    ```typescript
    openCallDialog(data: Phone) {
        this.eventBus.emit({ name: Events.CustomerCall, value: data });
    }
    ```
    
    > [!NOTE]
    > The event bus code can be found in the `eventbus.service.ts` file if you're interested in exploring it more. The full path to the file is `openai-msgraph-acs/client/src/app/core/eventbus.service.ts`.

1. The header component's `ngOnInit()` function subscribes to the `CustomerCall` event sent by the event bus and displays the phone call component. You can find this code in `header.component.ts`.

    ```typescript
    ngOnInit() {
        this.subscriptions.push(
            this.eventBus.on(Events.CustomerCall, (data: Phone) => {
                this.callVisible = true; // Show phone call component
                this.callData = data; // Set phone number to call
            })
        );
    }
    ```

1. Open `phone-call.component.ts` in your editor using the keyboard shortcuts mentioned earlier. The full path to the file is `openai-msgraph-acs/client/src/app/phone-call/phone-call.component.ts`. Take a moment to expore the code. Note the following key features:

    - Retrieves an Azure Communication Services access token by calling the `acsService.getAcsToken()` function in `ngOnInit()`;
    - Adds a "phone dialer" to the page. You can see the dialer by clicking on the phone number input in the header.
    - Starts and ends a call using the `startCall()` and `endCall()` functions respectively.

1. Before looking at the code that makes the phone call, let's examine how the ACS access token is retrieved and how phone calling objects are created. Locate the `ngOnInit()` function in   `phone-call.component.ts`.

    ```typescript
    async ngOnInit() {
        if (ACS_CONNECTION_STRING) {
            this.subscriptions.push(
                this.acsService.getAcsToken().subscribe(async (user: AcsUser) => {
                    const callClient = new CallClient();
                    const tokenCredential = new AzureCommunicationTokenCredential(user.token);
                    this.callAgent = await callClient.createCallAgent(tokenCredential);
                })
            );
        }
    }
    ```

    This function performs the following actions:

    - Retrieves an ACS access token by calling the `acsService.getAcsToken()` function.
    - Once the access token is retrieved, the code performs the following actions:
        - Creates a new instance of `CallClient` and `AzureCommunicationTokenCredential` using the access token.
        - Creates a new instance of `CallAgent` using the `CallClient` and `AzureCommunicationTokenCredential` objects. Later you'll see that `CallAgent` is used to start and end a call.

1. Open `acs.services.ts` and locate the `getAcsToken()` function. The full path to the file is `openai-msgraph-acs/client/src/app/core/acs.service.ts`. Note that the function makes an HTTP GET request to the API server to retrieve the ACS access token.

    ```typescript
    getAcsToken(): Observable<AcsUser> {
        return this.http.get<AcsUser>(API_BASE_URL + 'acstoken')
        .pipe(
            catchError(this.handleError)
        );
    }
    ```

1. An API server function named `createACSToken()` retrieves the access token and returns it to the client. It can be found in the `openai-msgraph-acs/server/typescript/acs.ts` file. 

    ```typescript
    const connectionString = process.env.ACS_CONNECTION_STRING as string;

    async function createACSToken() {
        if (!connectionString) return { userId: '', token: '' };

        const tokenClient = new CommunicationIdentityClient(connectionString);
        const user = await tokenClient.createUser();
        const userToken = await tokenClient.getToken(user, ["voip"]);
        return { userId: user.communicationUserId, ...userToken };    
    }
    ```

    This function performs the following actions:

    - Checks if an ACS `connectionString` value is available. If not, returns an object with an empty `userId` and `token`.
    - Creates a new instance of `CommunicationIdentityClient` and passes the `connectionString` value to it.
    - Creates a new user using `tokenClient.createUser()`.
    - Gets a token for the new user with the "voip" scope using `tokenClient.getToken()`.
    - Returns an object containing the `userId` and `token` values.

1. Now that you've seen how the token is retrieved, go back to `phone-call.component.ts` and locate the `startCall()` function. 

1. This function is called when **Call** is selected in the phone call component. It uses the `CallAgent` object (located in the `@azure/communication-calling` library) mentioned earlier to start a call. The `callAgent.startCall()` function accepts an object representing the number to call and the ACS phone number used to make the call.

    ```typescript
    startCall() {
        this.call = this.callAgent?.startCall(
            [{ phoneNumber: this.customerPhoneNumber }], {
            alternateCallerId: { phoneNumber: this.fromNumber }
        });
        console.log('Calling: ', this.customerPhoneNumber);
        console.log('Call id: ', this.call?.id);
        this.inCall = true;
    }
    ```

1. The `stopCall()` function is called when **Hang Up** is selected in the phone call component.

    ```typescript
    endCall() {
        if (this.call) {
            this.call.hangUp({ forEveryone: true });
            this.call = undefined;
            this.inCall = false;
        }
        else {
            this.hangup.emit();
        }
    }
    ```

    If a call is in progress, the `call.hangUp()` function is called to end the call. If no call is in progress, the `hangup` event is emitted to the parent component to change the button text from **Hang Up** to **Close**.

1. Before moving on to the next exercise, let's review the key concepts covered covered:

    - An ACS access token is retrieved from the API server using the `acsService.getAcsToken()` function. 
    - The token is used to create a `CallClient` and `CallAgent` object.
    - The `CallAgent` object is used to start and end a call by calling the `callAgent.startCall()` and `callAgent.hangUp()` functions respectively.

1. Now that you've learned how phone calling can be integrated into an application, let's switch our focus to using Azure Communication Services to send email and SMS messages.