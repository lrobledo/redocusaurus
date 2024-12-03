---
sidebar_position: 1
---

# Overview

Steps to get started using the Capstone API.
This guide explains the nature of the Capstone API, how to set up your basic authentication header,
and walks you through sending your first request.

If you need help please don't hesitate to contact out to our <a href='#contact-us'>integration support</a> team.
Team members are standing by, ready to help you get started.

## Capstone API core-concepts

### Base URLs

All endpoints are RESTful with the following base URLs:

- Sandbox: `https://api.capstonesandbox.com`
- Production: `https://api.capstone.com`

The **sandbox** environment allows you to send test requests without affecting live data.
After you have integrated with the sandbox environment,
you will be ready to integrate with the production environment by changing the authentication information and base URLs.

### Sending requests

The examples in our [API reference](/api) show how to send requests to the Capstone API using cURL,
but feel free to requests using your preferred HTTP client.
You can also download our [Postman Collection](https://app.getpostman.com)
or check out one of our [software development kits](#).

### Examples

The examples included in this documentation are on a Unix-based OS.
If you need help getting started with another operating system, please reach out to our <a href='#contact-us'>support</a> team.

## Authentication

Most requests in the Capstone API use basic authentication.
Basic authentication is a common authentication method in which you create an authentication header using your username and password.
(Before moving on, please make sure you have a Capstone [account](#) with a validated email address.) 

To create your authentication header, follow the steps below:

1. Create a username + password string

  Create a string with your Capstone username and password, separated by a colon.
  In the example below, the username is `me@mysite.com` and the password is `mypassword`.

```text
me@mysite.com:mypassword
```

2. **Encode your username and password**

   Base64 the string from step 1.
   You can use your preferred [encoding tool](https://www.base64encode.org/), as long as it is secure.

```bash
$ echo -n me@mysite.com:mypassword | base64
bWVAbXlzaXRlLmNvbTpteXBhc3N3b3Jk
```

3. **Create the authorization header**

   Prefix the value from **step 2** with the string "Authorization: Basic ".
   The result is your authentication header

```text
Authorization: Basic bWVAbXlzaXRlLmNvbTpteXBhc3N3b3Jk
```

4. **Send a request**

   Include your authentication header in every request to the Capstone API.

```bash
curl -X get https://api.capstonesandbox.com/v3/whoAmI \
  -H 'Accept: application/json' \
  -H 'Authorization: Basic bWVAbXlzaXRlLmNvbTpteXBhc3N3b3Jk'
```

### Your first request

To start getting familiar with our API, send a request to the [Who Am I](#who-am-i) endpoint.
A successful request will return information about the user whose credentials were used to create the basic authentication header

This information includes:
- First Name
- Last Name
- Username
- A list of the user's access rights

#### Example request

```bash
curl -X GET https://api.capstonesandbox.com/v3/whoAmI \
  -H 'Accept: application/json' \
  -H 'Authorization: Basic <Your basic authentication>'
```

#### Example 200 response

```bash
{
  "firstName": "Livia ",
  "lastName": "Alves",
  "userName": "liv.alves@example.com",
  "accessRights": {
    "role": "admin"
  },
  "dateLastModified": "2019-03-08T00:58:27.893Z",
  "dateCreated": "2018-09-27T14:27:39.626Z",
  "phone": "15555555555"
}
```

### Next steps

Depending on your workflow, you may wish to:
- Create a [new user](#)
- Change a user's [access rights](#)
- Create a [new location](#)
