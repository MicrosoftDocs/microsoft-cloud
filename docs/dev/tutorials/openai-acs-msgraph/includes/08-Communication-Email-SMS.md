<!-- markdownlint-disable MD041 -->

In addition to phone calls, Azure Communication Services can also be used to send email and SMS messages. This can be useful when you want to send a message to a customer or other user directly from the application.

In this exercise, you will:
- Explore how email and SMS messages can be sent from the application.
- Walk through the code to learn how the email and SMS functionality is implemented.

### Using the Email and SMS Features

1. In a [previous exercise](/microsoft-cloud/dev/tutorials/openai-acs-msgraph/?tutorial-step=5) you created an Azure Communication Services (ACS) resource and started the database, web server, and API server. You also updated the following values in the *.env* file.

    ```
    ACS_CONNECTION_STRING=<ACS_CONNECTION_STRING>
    ACS_PHONE_NUMBER=<ACS_PHONE_NUMBER>
    ACS_EMAIL_ADDRESS=<ACS_EMAIL_ADDRESS>
    CUSTOMER_EMAIL_ADDRESS=<EMAIL_ADDRESS_TO_SEND_EMAIL_TO>
    CUSTOMER_PHONE_NUMBER=<UNITED_STATES_BASED_NUMBER_TO_SEND_SMS_TO>
    ```

    Ensure that you've completed the [previous exercise](/microsoft-cloud/dev/tutorials/openai-acs-msgraph/?tutorial-step=5) before continuing.

1. Go back to the browser (*http://localhost:4200*), locate the datagrid, and select **Contact Customer** followed by **Email/SMS Customer** in the first row.

    :::image type="content" source="../media/acs-email-sms-customer.png" alt-text="Send an email or SMS message using ACS.":::

1. In the **Email/SMS** tab, perform the following tasks:

    - Enter an Email **Subject** and **Body** and select the **Send Email** button.
    - Enter an SMS message and select the **Send SMS** button.

    :::image type="content" source="../media/acs-email-sms-customer-dialog.png" alt-text="Email/SMS Customer dialog box.":::

1. Check that you received the email and SMS messages. As a reminder, the email message will be sent to the value defined for `CUSTOMER_EMAIL_ADDRESS` and the SMS message will be went to the value defined for `CUSTOMER_PHONE_NUMBER` in the *.env* file. If you weren't able to supply a United States based phone number to use for SMS messages you can skip that step.

    > [!NOTE]
    > If you don't see the email message in your inbox for the address you defined for `CUSTOMER_EMAIL_ADDRESS` in the *.env* file, check your spam folder.

### Exploring the Email Code

[!INCLUDE [Note-Open-Files-VS-Code](./tip-open-files-vs-code.md)]

1. Open the *customers-list.component.ts* file. The full path to the file is *openai-acs-msgraph/client/src/app/customers-list/customers-list.component.ts*.

1. When you selected **Contact Customer** followed by **Email/SMS Customer** in the datagrid, the `customer-list` component displayed a dialog box. This is handled by the `openEmailSmsDialog()` function in the *customer-list.component.ts* file.

    ```typescript
    openEmailSmsDialog(data: any) {
        if (data.phone && data.email) {
            let dialogData: EmailSmsDialogData = {
                prompt: '',
                title: `Contact ${data.company}`,
                company: data.company,
                customerName: data.first_name + ' ' + data.last_name,
                customerEmailAddress: data.email,
                customerPhoneNumber: data.phone
            }

            const dialogRef = this.dialog.open(EmailSmsDialogComponent, {
                data: dialogData
            });

            this.subscriptions.push(
                dialogRef.afterClosed().subscribe((response: EmailSmsDialogData) => {
                    console.log('SMS dialog result:', response);
                    if (response) {
                        dialogData = response;
                    }
                })
            );
        }
        else {
            alert('No phone number available.');
        }
    }
    ```

    The `openEmailSmsDialog()` function performs the following tasks:

    - Checks to see if the `data` object (which represents the row from the datagrid) contains a `phone` and `email` property. If it does, it creates a `dialogData` object that contains the information to pass to the dialog.
    - Opens the `EmailSmsDialogComponent` dialog box and passes the `dialogData` object to it.
    - Subscribes to the `afterClosed()` event of the dialog box. This event is fired when the dialog box is closed. The `response` object contains the information that was entered into the dialog box.

1. Open the *email-sms-dialog.component.ts* file. The full path to the file is *openai-acs-msgraph/client/src/app/email-sms-dialog/email-sms-dialog.component.ts*.

1. Locate the `sendEmail()` function:

    ```typescript
    sendEmail() {
        if (this.featureFlags.acsEmailEnabled) {
            // Using CUSTOMER_EMAIL_ADDRESS instead of this.data.email for testing purposes
            this.subscriptions.push(
                this.acsService.sendEmail(this.emailSubject, this.emailBody, this.getFirstName(this.data.customerName), CUSTOMER_EMAIL_ADDRESS /* this.data.email */)
                .subscribe(res => {
                    console.log('Email sent:', res);
                    if (res.status) {
                        this.emailSent = true;
                    }
                })
            );
        }
        else {
            this.emailSent = true; // Used when ACS email isn't enabled
        }
    }
    ```

    The `sendEmail()` function performs the following tasks:

    - Checks to see if the `acsEmailEnabled` feature flag is set to `true`. 
    - If `acsEmailEnabled` is true, the `acsService.sendEmail()` function is called and the email subject, body, customer name, and customer email address are passed. Because the database contains sample data, the `CUSTOMER_EMAIL_ADDRESS` value from the `.env` file is used instead of `this.data.email`. In a real-world application the `this.data.email` value would be used.
    - Subscribes to the `sendEmail()` function in the `acsService` service. This function returns an RxJS observable that contains the response from the client-side service.
    - If the email was sent successfully, it sets the `emailSent` property to `true`.

1. To provide better code encapsulation and reuse, client-side services such as *acs.service.ts* are used throughout the application. This allows all ACS functionality to be consolidated into a single place.

1. Open *acs.service.ts* and locate the `sendEmail()` function. The full path to the file is *openai-acs-msgraph/client/src/app/core/acs.service.ts*.

    ```typescript
    sendEmail(subject: string, message: string, customerName: string, customerEmailAddress: string) : Observable<EmailSmsResponse> {
        return this.http.post<EmailSmsResponse>(API_BASE_URL + 'sendemail', { subject, message, customerName, customerEmailAddress })
        .pipe(
            catchError(this.handleError)
        );
    }
    ```

    The `sendEmail()` function in `AcsService` performs the following tasks:

    - Calls the `http.post()` function and passes the email subject, message, customer name, and customer email address to it. The `http.post()` function returns an RxJS observable that contains the response from the API call.
    - Handles any errors returned by the `http.post()` function using the RxJS `catchError` operator.

1. Now let's examine how the application interactions with the ACS email feature. Open the *acs.js* file. The full path to the file is *openai-acs-msgraph/server/typescript/acs.js* and locate the `sendEmail()` function.

1. The `sendEmail()` function performs the following tasks:

    - Creates a new `EmailClient` object and passes the ACS connection string to it (this value is retrieved from the `ACS_CONNECTION_STRING` environment variable).

        ```typescript
        const emailClient = new EmailClient(connectionString);
        ```

    - Creates a new `EmailMessage` object and passes the content and recipient information.

        ```typescript
        const msgObject: EmailMessage = {
            senderAddress: process.env.ACS_EMAIL_ADDRESS as string,
            content: {
                subject: subject,
                plainText: message,
            },
            recipients: {
                to: [
                {
                    address: customerEmailAddress,
                    displayName: customerName,
                },
                ],
            },
        };
        ```

    - Sends the email using the `emailClient.beginSend()` function and returns the response. Although the function is only sending to one recipient in this example, the `beginSend()` function can be used to multiple recipients as well.

        ```typescript
        const poller = await emailClient.beginSend(msgObject);
        ```

    - Waits for the `poller` object to signal it's done and sends the response to the caller.


### Exploring the SMS Code

1. Go back to the *email-sms-dialog.component.ts* file that you opened earlier. The full path to the file is *openai-acs-msgraph/client/src/app/email-sms-dialog/email-sms-dialog.component.ts*.

1. Locate the `sendSms()` function:

    ```typescript
    sendSms() {
        if (this.featureFlags.acsPhoneEnabled) {
            // Using CUSTOMER_PHONE_NUMBER instead of this.data.customerPhoneNumber for testing purposes
            this.subscriptions.push(
                this.acsService.sendSms(this.smsMessage, CUSTOMER_PHONE_NUMBER /* this.data.customerPhoneNumber */).subscribe(res => {
                    if (res.status) {
                    this.smsSent = true;
                    }
                })
            );
        }
        else {
            this.smsSent = true;
        }
    }
    ```

    The `sendSMS()` function performs the following tasks:

    - Checks to see if the `acsPhoneEnabled` feature flag is set to `true`. 
    - If `acsPhoneEnabled` is true, the `acsService.SendSms()` function is called and the SMS message and customer phone number are passed. Because the database contains sample data, the `CUSTOMER_PHONE_NUMBER` value from the `.env` file is used instead of `this.data.customerPhoneNumber`. In a real-world application the `this.data.customerPhoneNumber` value would be used.
    - Subscribes to the `sendSms()` function in the `acsService` service. This function returns an RxJS observable that contains the response from the client-side service.
    - If the SMS message was sent successfully, it sets the `smsSent` property to `true`.

1. Open *acs.service.ts* and locate the `sendSms()` function. The full path to the file is *openai-acs-msgraph/client/src/app/core/acs.service.ts*.

    ```typescript
    sendSms(message: string, customerPhoneNumber: string) : Observable<EmailSmsResponse> {
        return this.http.post<EmailSmsResponse>(API_BASE_URL + 'sendsms', { message, customerPhoneNumber })
        .pipe(
            catchError(this.handleError)
        );
    }  
    ```

    The `sendSms()` function in `AcsService` performs the following tasks:

    - Calls the `http.post()` function and passes the message and customer phone number to it. The `http.post()` function returns an RxJS observable that contains the response from the API call.
    - Handles any errors returned by the `http.post()` function using the RxJS `catchError` operator.

1. Finally, let's examine how the application interactions with the ACS SMS feature. Open the *acs.js* file. The full path to the file is *openai-acs-msgraph/server/typescript/acs.js* and locate the `sendSms()` function.

1. The `sendSms()` function performs the following tasks:

    - Creates a new `SmsClient` object and passes the ACS connection string to it (this value is retrieved from the `ACS_CONNECTION_STRING` environment variable).

        ```typescript
        const smsClient = new SmsClient(connectionString);
        ```

    - Calls the `smsClient.send()` function and passes the ACS phone number (`from`), customer phone number (`to`), and SMS message:

        ```typescript
        const sendResults = await smsClient.send({
            from: process.env.ACS_PHONE_NUMBER as string,
            to: [customerPhoneNumber],
            message: message
        });
        return sendResults;
        ```

    - Returns the response to the caller.

1. You can learn more about ACS email and SMS functionality in the following articles:

    - [Email in Azure Communication Services](/azure/communication-services/concepts/email/email-overview)
    - [SMS in Azure Communication Services](/azure/communication-services/concepts/sms/concepts)



