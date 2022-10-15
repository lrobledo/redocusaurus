---
sidebar_position: 1
---

# Getting Started
### Requirements

To get started using the Capstone API you will need:
- **Capstone credentials:** To request credentials, send us a message at sandbox@Capstone.com
- **An HTTP client:** Our [API reference](/api) includes cURL examples.
  You can also download our [Postman Collection](https://app.getpostman.com)
  or use your preferred HTTP client.

### Base URLs and Environments

All endpoints are RESTful with the following base URLs:

Sandbox: `https://api.capstonesandbox.com`

Production: `https://api.capstone.com`

The **sandbox** environment allows you to run test transactions without affecting live data.

Once you have integrated with the sandbox environment you are ready to integrate to production with minimal changes.
(Required updates include [authentication information](#basic-authentication) and encryption keys.)


### Your First Request

A good place to start getting familiar with our API is by sending a request to the [Who Am I](#who-am-i) endpoint.

A successful request to this endpoint will return information about the user whose credentials have been used to authenticate the request,
including:
- First Name
- Last Name
- Username
- A list of merchants to which the user has access rights

To send a request, follow the steps below:

1. Authenticate via [basic authentication](#basic-authentication).

2. Send a `GET` request to the [Who Am I](#who-am-i) endpoint.

<pre class="code-header example-request">Example Request</pre>
```javascript
curl -X GET https://api.Capstonesandbox.com/user/v3/account/whoAmI \
  -H 'Accept: application/json' \
  -H 'Authorization: Basic <Your Basic Auth>'
```

<pre class="code-header example-response">Example 200 Response</pre>
```javascript
{
  "cognitoSub": "g52679a2-tan5-4a56-b6gc-2592aedd373e",
  "firstName": "John",
  "lastName": "Doe",
  "userName": "jdoe@yourwebsite.com",
  "accessRights": {
    "merchantIds": {
      "100039": "A"
    },
    "role": "A"
  },
  "dateLastModified": "2019-03-08T00:58:27.893Z",
  "dateCreated": "2018-09-27T14:27:39.626Z",
  "enabled": true,
  "phone": "15555555555",
  "notifications": false
}
```

### Next steps

Once you have successfully made your first request, you are ready to move on.
Depending on your workflow, you may wish to:
- Load a [retail iframe](#retail-header)
- Load an [e-commerce iframe](#e-commerce-header)
- Use an [alternative payment method](#alternative-payment-methods-header)
- Configure [webhooks](#webhooks) to notify you when transaction events take place
- Retrieve a list of [transactions](#transactions)
- [Dispute](#dispute-chargeback) a chargeback
