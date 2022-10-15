---
sidebar_position: 5
---

# Subscriptions
Capstone's [subscription service](#subscriptions-header) allows you to run recurring transactions.
These can be in the form of basic [subscriptions](#subscriptions) or [pay plans](#pay-plans).

## Subscriptions {#basic-subscriptions}
Subscriptions set transactions to run for a specified amount on a predefined schedule.
When you [create a subscription](#create-subscription)
you will set the following parameters:
- **Amount**: (`payment.data.amount`) The amount that will be charged during each transaction
- **Interval**: (`schedule.interval`) The schedule's time period. May be days, weeks, months or years
- **Interval count**: (`schedule.intervalCount`) The number of intervals that will pass between transactions.
  See the [table below](#subscription-schedule-examples) for examples of how the interval and interval count create the subscription schedule

- **Initial billing period *(optional)***: If desired, you may set an initial billing period
  during which the transactions will be run for an amount different than the subscription amount.
  (For example, during a discounted trial period.)
  To set an initial billing period, include the following parameters:
    - **Initial Billing Period End Date**: (`schedule.dateInitialBillingEnd`) The date at which the initial billing period will end.
      Transactions prior to this date will be run for the initial billing amount.
      Transactions after this date will be run for the subscription amount.
    - **Initial billing Amount**: (`schedule.initialBillingAmount`) The transaction amount during the initial billing period

#### Subscription Schedule Examples
| Interval Count | Interval | Explanation                             |
|----------------|----------|-----------------------------------------|
| 30             | Day      | Transactions will run every 30 days     |
| 4              | Week     | Transactions will run every 4 weeks     |
| 2              | Month    | Transactions will run every other month |

### Create a Subscription
1. **Determine your Settings**

   Determine whether to include an initial billing period
   and the proper subscription settings.
   (These will generally be pre-determined by the shopper's selections on your checkout page.)

2. **Send a Request to Capstone**

   Send a POST request to the [Create Subscription](#create-subscription) endpoint.
   Include the billing amounts and subscription settings as determined in **step 1**.

The example below will create a recurring subscription that will run a transaction for 19.99 USD every other week.

#### Basic Subscription
```
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
    }
  },
  "schedule": {
    "interval": "week",
    "intervalCount": 2
  }
}'
```

The example below will create a recurring subscription that will run a transaction once a month.
The transaction amount will be 15 USD until October 5, 2021.
After that the transaction amount will be 25 USD.

#### Subscription with initial billing period
```
curl -X POST https://api.Capstonesandbox.com/subscription/v3 \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -H 'Authorization: Basic <Your Basic Auth>'
  -d '{
  "payment": {
    "data": {
      "amount": 25,
      "currency": "USD",
      "customer": {
        "customerRef": "RP006"
      }
    },
    "tokenex": {
      "token": "6ee140a0-05d1-4958-8325-b38a690dbb9d"
    }
  },
  "schedule": {
    "interval": "month",
    "intervalCount": 1,
    "dateInitialBillingEnd": "2021-10-05",
    "initialBillingAmount": 15
  }
}'
```

**Note:** If the initial transaction is declined, the subscription will not be created.
It must be re-created with a new card token.

3. **Configure Webhooks *(Optional)***

   If desired,
   you may configure webhooks to be sent every time a transaction is run through the
   [subscription](#subscription-event) service.
   See the [Webhooks](#webhooks) tutorial for step-by-step instructions on how to [configure](#configure-webhooks) and [receive](#receive-webhooks) webhooks.

   **Note:** You will continue to receive webhooks for any previously configured [transaction events](#transaction-events).

### Example Request
```
curl -X POST https://api.Capstonesandbox.com/webhook/v3/config \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -H 'Authorization: Basic <Your Basic Auth>'
  -d '{
  "merchantId": "100039",
  "webhooks": {
    "SUBSCRIPTION_DETAILS": {
      "url": "https://your-company-webhook-url-here.com"
    }
  }
}'
```

## Pay Plans
Pay plans set recurring transactions to run until the pre-specified balance is paid in full.
To create a pay plan, [create a subscription](#create-subscription) and include the following parameters in your request:
- **Balance**: (`schedule.balance`) The total amount to be charged over the course of the pay plan
- **Interval**: (`schedule.interval`) The pay plans's time period. May be days, weeks, months or years
- **Interval count**: (`schedule.intervalCount`) The number of intervals that will pass between transactions.
  See the [table below](#subscription-schedule-examples) for examples of how the interval and interval count create the subscription schedule
- **Amount *(optional)***: (`payment.data.amount`) The amount to be charged during each transaction
- **Duration *(optional)***: (`schedule.duration`) The duration of the subscription.
  This is the number of transactions that will run before the pay plan terminates.
  If amount is not provided the duration will also be used to calculate the amount to be charged during each transaction

**Note:** You must provide either an amount or a duration.
If you provide both, amount will be used, duration will be ignored.

### Create a Pay Plan
1. **Determine your Settings**

   Determine the pay plan balance and schedule.
   Also determine whether you would like to provide an amount or a duration (not both).
   These may be pre-determined by the shopper's selections on your checkout page.

2. **Send a Request to Capstone**

   Send a POST request to the [Create Subscription](#create-subscription) endpoint.
   Include the balance, schedule and amount/duration as determined in **step 1**.

The example below will create a pay plan with a balance of 100 USD.
It will run a transaction once a month for 10 USD.
After the 10th transaction, the balance will be paid in full and the pay plan with terminate.

#### PAY PLAN WITH TRANSACTION AMOUNT
```
curl -X POST https://api.Capstonesandbox.com/subscription/v3 \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -H 'Authorization: Basic <Your Basic Auth>'
  -d '{
  "payment": {
    "data": {
      "amount": 10,
      "currency": "USD",
      "customer": {
        "customerRef": "RP006"
      }
    },
    "tokenex": {
      "token": "6ee140a0-05d1-4958-8325-b38a690dbb9d"
    }
  },
  "schedule": {
    "balance": 100,
    "interval": "month",
    "intervalCount": 1
  }
}'
```

The example below will create a pay plan with a balance of 100 USD that will run every month for 5 months.
In order for the balance to be paid in full, each transaction will automatically be run for 20 USD.

#### PAY PLAN WITH DURATION
```shell
curl -X POST https://api.Capstonesandbox.com/subscription/v3 \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -H 'Authorization: Basic <Your Basic Auth>'
  -d '{
  "payment": {
    "data": {
      "currency": "USD",
      "customer": {
        "customerRef": "RP006"
      }
    },
    "tokenex": {
      "token": "6ee140a0-05d1-4958-8325-b38a690dbb9d"
    }
  },
  "schedule": {
    "balance": 100,
    "duration": 5,
    "interval": "month",
    "intervalCount": 1
  }
}'
```

3. **Configure Webhooks *(Optional)***

   If desired,
   you may configure webhooks to be sent every time a transaction is run through the
   [subscription](#subscription-event) service.
   See the [Webhooks](#webhooks) tutorial for step-by-step instructions on how to [configure](#configure-webhooks) and [receive](#receive-webhooks) webhooks.

   **Note:** You will continue to receive webhooks for any previously configured [transaction events](#transaction-events).

### EXAMPLE REQUEST
```
curl -X POST https://api.Capstonesandbox.com/webhook/v3/config \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -H 'Authorization: Basic <Your Basic Auth>'
  -d '{
  "merchantId": "100039",
  "webhooks": {
    "SUBSCRIPTION_DETAILS": {
      "url": "https://your-company-webhook-url-here.com"
    }
  }
}'
```