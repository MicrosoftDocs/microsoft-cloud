Microsoft Graph exposes hundreds of endpoints that allow you to tap into data and insights in Microsoft 365. To use these API endpoints, you need to request a correct set of permissions. 

If you work on a large solution that uses many endpoints, it can be difficult to build the exact list of minimal permissions for your application.

To detect the minimal Microsoft Graph API permissions that your app requires:

1. [Start recording](./Record-and-export-proxy-activity).
1. Issue requests as normal from your app. 
1. [Stop recording](./Record-and-export-proxy-activity).

A list of minimal permissions will be returned in the activity summary based on the requests made. 

For example:

```sh
Retrieving minimal permissions for:
- GET /me
- GET /users/{users-id}/calendars

Minimal permissions:
User.Read, Calendars.Read
```

By default, only `Delegated` permissions are returned in the summary.

To return `Application` permissions, update the `minimalPermissionsPlugin` configuration block in the [m365proxyrc](./m365proxyrc) file to:

```json
"minimalPermissionsPlugin": {
  "type": "application"
},
```
