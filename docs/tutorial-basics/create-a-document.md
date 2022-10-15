---
sidebar_position: 2
---

# Decline Recovery (BETA)

**Note: This feature is in beta.**
Decline Recovery is a dynamic tool that helps ensure you don't lose revenue due to payment declines.
When the Decline Recovery service is enabled,
Capstone will automatically retry declined transactions at a more desirable date and time
or with more complete customer information as available (for example, a billing address).
You can use Capstone's Decline Recovery service when running card transactions with [the API](#decline-recovery-api)
or card transactions run through the [subscription](#subscriptions-header) service.

**Notes:**
- Decline Recovery does not immediately determine whether the payment will go through at a later date.
  It should not be used for payments for which the customer will be waiting for payment confirmation,
  such as through the payment iframe or your own form. <!-- textlint-disable -->
- Decline Recovery will **not** attempt to retry transactions classified as hard declines.
  In general, hard declines require the issuer or cardholder to rectify outstanding issues.
  (See [FlexPay's documentation](https://docs.flexpay.io/) for more information regarding hard and soft declines.)

<!-- WITH THE API -------------------------------------------------------------------------------------------------------------------->
## With The API {#decline-recovery-api}
You may enable Decline Recovery on transactions run directly through the Capstone API.

1. **Configure Your Account**

   Contact [integration support](#contact-us) to enable Decline Recovery on your merchant account.

2. **Configure Webhooks**

   In order to be notified when a previously declined transaction is successfully processed,
   you must [configure webhooks](#configure-webhooks) for `TRANSACTION_AUTHORIZED`, `TRANSACTION_CAPTURED` and `TRANSACTION_SETTLED` events.
   See the [Webhooks](#webhooks) tutorial for instructions on how to [configure](#configure-webhooks) and [receive](#receive-webhooks) webhooks.
   You may also wish to configure webhooks for other transaction [events](#event-types) at this time.

   (Skip this step if you have already configured webhooks for all desired events on your account.)

3. **Post Card Information to Capstone**

   Post payment details along with a previously saved card token to the [Run Card Transaction](#run-card-transaction) endpoint.
   Include the object `"processingOptions": { "retryOnSoftDecline": true }`
   in the body of your request.

   If desired you may also choose to run an auth only transaction.

#### Example Request
```javascript
curl -X POST https://api.Capstonesandbox.com/pay/v3/process \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -H 'Authorization: Basic <Your Basic Auth>'
  -d '{
  "data": {},
  "tokenex": {
    "token": "6ee140a0-05d1-4958-8325-b38a690dbb9d"
  },
  "processingOptions": {
    "retryOnSoftDecline": true
  }
}'
```

Now, any successful transactions will be run as usual.
Soft declines will be scheduled to be rerun at a later time.

4. **Await Success Confirmation**

   Each time a transaction is retried Capstone will send a webhook (see **step 2**) with one of the following event types:
   `TRANSACTION_AUTHORIZED`, `TRANSACTION_CAPTURED` and `TRANSACTION_SETTLED`.
   The webhook body will include the transaction status (`data.transactionStatus`), letting you know whether the transaction was successful.

<!-- SUBSCRIPTIONS -------------------------------------------------------------------------------------------------------------------->
## Through the Subscription Service {#decline-recovery-subscription}
You may enable Decline Recovery on transactions run through the Capstone [Subscription Service](#subscriptions).

1. **Configure Your Account**

   Contact [integration support](#contact-us) to enable Decline Recovery on your merchant account.

2. **Configure Webhooks**

   In order to be notified when a previously declined transaction is successfully processed,
   you must configure webhooks for `TRANSACTION_AUTHORIZED`, `TRANSACTION_CAPTURED` and `TRANSACTION_SETTLED` events.
   See the [Webhooks tutorial](#webhooks) for instructions on how to configure and receive webhooks.
   You may also wish to configure webhooks for other transaction events at this time.

3. **Create a Subscription or Pay Plan**

   Follow **steps 1-2** of the [Create a Subscription](#create-a-subscription) or [Create a Pay Plan](#create-a-pay-plan) tutorial.
   Set the parameter `payment.processingOptions.retryOnSoftDecline` to `true` in the body of your request.

#### Example Request
```javascript
curl -X POST https://api.Capstonesandbox.com/subscription/v3 \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -H 'Authorization: Basic <Your Basic Auth>'
  -d '{
  "payment": {
    "data": {
      "amount": 19.99,
      "currency": "USD",
      "customer": {
        "customerRef": "RP006"
      }
    },
    "tokenex": {
      "token": "6ee140a0-05d1-4958-8325-b38a690dbb9d"
    },
    "processingOptions": {
      "retryOnSoftDecline": true
    }
  },
  "schedule": {
    "interval": "week",
    "intervalCount": 2
  }
}'
```

If the initial transaction is declined, the subscription will not be created.
It must be re-created with corrected information (such as new card token, corrected billing information, etc.)
Any future transactions that are declined will be rerun at a later date and time.
(Up to 11 times within a 30 day period.)

**Note:** Subscriptions run with Decline Recovery have a minimum interval of 29 days.

4. **Monitor Webhooks**

   Each time a transaction is retried Capstone will send a webhook (see **step 2**) with one of the following event types:
   `TRANSACTION_AUTHORIZED`, `TRANSACTION_CAPTURED` and `TRANSACTION_SETTLED`.
   The webhook body will include the transaction status (`data.transactionStatus`), letting you know whether the transaction was successful.