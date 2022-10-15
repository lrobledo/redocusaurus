---
sidebar_position: 2
---

# Frequently Asked Questions
## Error Codes

### Q: I keep receiving the error: '442 Invalid one time use token'. Why is my token not working? {#faq-442-error}
Once a [one-time-use token](#one-time-use-token) has been used to submit an iframe it cannot be used to load another.
You must request a new one-time-use token prior to submitting each iframe.


### Q: Why am I getting a 403 error when I send a request for a one-time-use token? I know my credentials are correct. {#faq-403-error}
To request a one-time-use token you must send a `POST` request.
Sending a `GET` request will result in a `403 Forbidden` error.

### Q: I'm getting a 440 error when I try to specify which merchant to process with. {#faq-440 error}

To troubleshoot this issue, follow the steps below:

1. Make sure there are no typos when you enter the merchant ID.
2. Check that you have access to this merchant.
   You can do this by logging into the [Capstone Dashboard](https://dashboard.Capstone.com).
   If you do not see your desired merchant in the merchant selector dropdown, contact CMS Customer Service
3. Make sure you are sending the variable `merchantId` as a string and not as an integer.

### Q: Why do I keep receiving a 431 'Error from Kount' error while testing?

Please note that Kount verification will be performed by default
when [saving a card token](#save-card-token) or [running a card transaction](#run-card-transaction).
To solve for unexpected 431 errors while testing your integration, try either of the following solutions:

- Make sure your tests do not include any of the values shown [here](#test-kount-verification)
- Disable Kount verification by including the parameter`"processingOptions": { "checkFraud": false }` in the body of your request.
  (See the [Enable Kount Verification](#enable-kount-verification) tutorial for reference.)

### Q: Why am I getting an error saying `Authorization header requires 'Credential' parameter`?

The following is a common AWS error:
`
{
"message": "Authorization header requires 'Credential' parameter.
Authorization header requires 'Signature' parameter. Authorization header
requires 'SignedHeaders' parameter. Authorization header requires
existence of either a 'X-Amz-Date' or a 'Date' header. Authorization=Basic
Og=="
}
`

To debug it, please try the following:
<!-- textlint-disable stop-words-->
- Check that your HTTP method is correct.
  (For example, if the HTTP method is set to PUT when it should be POST you may receive this error)
<!-- textlint-enable-->
- Check that your request includes all required path and query parameters
- Check your request for typos in the URL

If you still cannot resolve the error, please contact [integration support](#contact-us).

## Testing
### Q: Can I test the iframes before integrating? {#faq-test-iframes}

Yes! Once you have a Capstone account, you can use our [Sandbox Tester](https://tester.Capstonesandbox.com).

### Q: Are there any cards I can test with? {#faq-test-cards}

Yes. Test cards vary by implementation. Contact [integration support](#contact-us) for test cards specific to your needs.

**Note:** If you are testing with the Processing Option `verifyCvc: true` you *must* set the CVC equal to 111,
otherwise you will receive a `437 Invalid CVC` error.

## Other
### Q: All payments are coming back as `pending`, are these actually successful? Can I mark the order as paid and ship product? {#faq-pending}

The status of `pending` means the transaction is approved but the batch has not yet settled.

Settled transactions will show a status of `authorized`.

You can be assured that any `pending`/`authorized` transactions will settle without any issue.

**Note:** Batch close time is typically at 11pm PDT.

See the [Transaction Status](#transaction-status) table for more information.

### Q: Is there a joint auth & capture method? {#faq-auth-capture}

Yes! The endpoint is called [Run Card Transaction](#run-card-transaction).
When you send a request to this endpoint, by default the transaction will be authorized and captured.

If you wish to run an auth only transaction, send `isAuthOnly: true` in the body of your request.

### Q: I see two custom field entries, what are those for? {#faq-custom-fields}

The two custom fields provided are optional for partners to use for reconciliation and/or reporting purposes.

### Q: Is it possible to run an auth only using the iframe and then capture through the API? {#faq-auth-only-iframe}

Yes, you can use the Capstone iframe in AuthOnly mode then capture the sale later if that is best for your workflow.

To run an auth only in the iframe,
include `isAuthOnly: true` the body of your request when you retrieve a [one-time-use token](#one-time-use-token).

You can then capture using the [Capture Transaction](#capture-transaction) endpoint of the API or through Capstone dashboard.

### Q: Can I host my localization/translation JSON file as a GitHub gist? {#faq-github-gist}
Yes, you can host your sandbox localization/translation file anywhere you choose.

To use a GitHub gist to host your translation file, follow the steps below:
1. [Create a GitHub gist](https://docs.github.com/en/github/writing-on-github/editing-and-sharing-content-with-gists/creating-gists) containing your translation JSON file
    - **Note:** The gist must be public in order for Capstone to access it
2. Copy the gist's URL. It will look like this:

   `https://gist.github.com/your-github-username/8bdeaf3e8bc8c473d06b82ad56dd1c7d`

3. Append `/raw` to end of the URL from step 2:

   `https://gist.github.com/your-github-username/8bdeaf3e8bc8c473d06b82ad56dd1c7d/raw`
    - This allows Capstone to access the raw JSON file hosted in your gist repository
4. Proceed to **step 4** of the [Iframe Label Translation](#label-translation) tutorial

**Note:** These steps do not apply to the production environment.
Production translation/localization files must be hosted by Capstone.
See **step 3** of the [Iframe Label Translation](#label-translation) tutorial for more information.