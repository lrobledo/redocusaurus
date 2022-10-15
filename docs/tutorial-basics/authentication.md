---
sidebar_position: 1

---

# Authentication
How to use Basic Authentication to authenticate each request to the Capstone API.

## Basic Authentication
To authenticate with basic authentication you will need a Capstone username and password.
Please contact <a href='#contact-us'>integration support</a> to create a Capstone account.

1. **Encode Your Username and Password**

   Base 64 encode your Capstone username and password with no spaces, separated by a colon.

#### Example
```bash
$ echo -n myname@Capstone.com:mypassword | base64
bXluYW1lQG5leGlvaHViLmNvbTpteXBhc3N3b3Jk
```

2. **Create the Authorization Header**

   Prefix the value from **step 1** with the string "Basic ".

#### Example
```
Authorization: Basic bXluYW1lQG5leGlvaHViLmNvbTpteXBhc3N3b3Jk
```

3. **Send a Request**

   Include the string from **step 2** in authorization header of your API request.

#### Example
```bash
curl -X get https://api.Capstonesandbox.com/user/v3/account/whoAmI \
  -H 'Accept: application/json' \
  -H 'Authorization: Basic bXluYW1lQG5leGlvaHViLmNvbTpteXBhc3N3b3Jk'
```

## One-time-use Tokens {#authentication-via-one-time-use-token}
### E-commerce {#e-commerce-one-time-use-token}
You will need a one-time-use token to load any e-commerce iframes or to [save a card token via the API](#save-card-token).

To get a one-time-use token,
send a request to the [E-Commerce One-time-use Token](#one-time-use-token) endpoint.
Authenticate using [basic authentication](#basic-authentication).
Include any information you wish to pass along to the applicable iframe or popup in the body of your request.
See the [E-commerce One-time-use Token](#one-time-use-token) API Reference for a complete list of parameters.

### Alternative Payment Methods {#alternative-payment-method-one-time-use-token}
You will need a one-time-use token to load an Alternative Payment Method (APM) Iframe.
To obtain an alternative payment method one-time-use token,
send a request to the [One-time-use Token (APM)](#apm-one-time-use-token) endpoint.
Authentication using [basic authentication](#basic-authentication).
Include any information you wish to pass along to the iframe in the body of your request.
See the [retail UI options](#ui-options) section for a complete list.

#### Notes
- **CORS requires that every request for a one-time-use token must be sent from a server.**
  If you attempt to send a request a frontend you will receive an error
- One-time-use tokens for [e-commerce](#e-commerce-header) iframes and [alternative payment methods](#alternative-payment-methods-header) are *not* interchangeable
- Not all body parameters that *may* be included in the body of a request for a one-time-use token apply to every iframe or popup
- Each one-time-use token can only be used to submit a single form
- Each one-time-use token expires after one hour

