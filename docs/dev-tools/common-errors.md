---
sidebar_position: 1
---

# Handling Errors and Declines

For most situations you will encounter a handful of error and success codes including 200, 401, 404, 400 and 500.
The most common responses will be:

- **200: Operation was successful**

- **4xx: Operation failed** (In the case of [payments](#payments-api-reference), a 4xx status code means the payment was not processed)

**Note:** Due to PCI regulations error messages may often be intentionally ambiguous.
Please contact us if you wish to discuss your error messages, or see our list of [common errors](#common-errors).

Errors in the payment forms can be handled by adding an event listener to the window.

#### Example
```javascript
window.addEventListener('message', function messageListener(event) {
    if (event.origin === iframeDomain) {
        if (event.data.event === 'error' || event.data.event === 'success' || event.data.event === 'processed') {
        // handle success or error messages
        }
        if (event.data.event === 'loaded') {
        //handle form loaded state
        }
        if (event.data.event === 'cardSaved') {
        //handle cardSaved
        }
        console.log(event.data.event);
    }
});
```

## Common Errors
Error |	Description {style="width:50%"}                                           | Status Code
------|---------------------------------------------------------------------------|-------------
404   | The requested item is not found                                           | 404
404   | Merchant not found or invalid merchant configuration                      | 404
409   | No merchants enabled for processing on this account for selected currency | 409
431   | Kount has identified a possible risk with the transaction                 | 400
432   | Invalid currency                                                          | 400
433   | Invalid gateway                                                           | 400
434   | Invalid TokenEx configuration                                             | 400
435   | [Unable to process with gateway](#gateway-declines)                       | 400
436   | Unable to capture void or refund                                          | 400
436   | Missing required fields                                                   | 400
437   | Invalid CVC                                                               | 400
438   | Invalid request                                                           | 400
439   | Unable to load TokenEx                                                    | 500
440   | Insufficient access                                                       | 401
441   | Amount needs to be a number                                               | 400
443   | Verify AVS Failed                                                         | 400
5xx   | Generic server error                                                      | 500