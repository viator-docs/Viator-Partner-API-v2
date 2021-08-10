# Booking confirmation types

**Note**: This section applies to merchant partners only.

A product's booking confirmation type indicates whether the booking will be confirmed or rejected immediately and automatically; or, whether it will remain in a 'pending' state until actioned manually by the supplier.

The product's booking confirmation type can be identified by the value of `bookingConfirmationSettings.confirmationType` in the product content response. There are three confirmation types: **instant**, **manual** and **instant-then-manual**, represented by the `confirmationType` values `"INSTANT"`, `"MANUAL"` and `"INSTANT_THEN_MANUAL"`, respectively.

## Booking confirmation type in the API

### Instant confirmation:

Instant confirmation products are confirmed automatically at the time of booking, and the customer should be charged immediately.

E.g. (product-code: 5010SYDNEY):

```javascript
"bookingConfirmationSettings": {
  "bookingCutoffType": "CLOSING_TIME",
  "bookingCutoffInMinutes": 0,
  "confirmationType": "INSTANT"
}
```

### Manual confirmation

Manual confirmation products are those that only operate at the discretion of the supplier, who must confirm or reject each booking request manually. Once the booking is requested, it will remain in a 'pending' state until actioned by the supplier; or, if no action is taken by the supplier, the time-out period (72 hours) is exceeded. The customer should only be charged once confirmation is received.

E.g. (product-code: 161500P1):

```javascript
"bookingConfirmationSettings": {
    "bookingCutoffType": "START_TIME",
    "bookingCutoffInMinutes": 1440,
    "confirmationType": "MANUAL"
}
```

### Instant-then-manual confirmation

Instant-then-manual confirmation products behave like instant confirmation products up until a certain time prior to the product's starting time. From that point on, the product will require manual confirmation by the supplier.

E.g. (product-code: 100006P10):

```javascript
"bookingConfirmationSettings": {
  "bookingCutoffType": "FIXED_TIME",
  "bookingCutoffInMinutes": 1440,
  "confirmationType": "INSTANT_THEN_MANUAL",
  "manualConfirmationPeriod": 10080,
  "bookingCutoffFixedTime": "00:01:00"
```

The `manualConfirmationPeriod` indicates when the product changes from instant to manual confirmation. In the example above, this period is 10,080 minutes (7 days). Therefore, this product will be confirmed instantly if the booking is made more than 7 days in advance of the start time; if it is less than 7 days, it will require manual confirmation.

## How to support manual confirmation products

In order to support manual confirmation products on your site, you will need to introduce support for an asynchronous flow that monitors the booking's status, determines when its status changes and notifies the customer when their booking has been confirmed or rejected by the supplier.

You will need to:

- Create back-end logic that periodically checks the status of pending bookings using the [/bookings/status](../../../openapi/reference/operation/bookingsStatus) endpoint (we recommend polling no more than once every 3 minutes)
- Establish and support an extra, email-based communications channel with your customers
- Create email templates for the various scenarios that may arise
- Ensure that your platform's front end accommodates the extra steps required in the booking process
- Ensure that customers clearly understand that they are booking a product that will not be confirmed or charged immediately

### Product detail page

Managing customer expectations is a key factor in supporting manual confirmation products on your booking platform. 

Make clear mention on the product detail page that confirmation will not be received immediately, but rather within 72 hours of making the booking.

You are free to include this note in the **Additional Info** section, or elsewhere on your product display page; for example:

<figure>
    <img src="https://docs.viator.com/partner-api/resources/merchant/technical/img/additional-info-on-request-clause.jpg" alt="Manual confirmation info displayed on a product page on the Viator site"/>
    <figcaption>Manual confirmation info displayed on a product page on the Viator site
     </figcaption>
</figure>

### Check-out

As you will only charge the customer's credit card once the booking is confirmed by the product's supplier, it's best to display a message to this effect at a prominent point of the check-out flow for all manual confirmation products.

<figure>
    <img src="https://docs.viator.com/partner-api/resources/merchant/technical/img/checkout-flow-confirmation-info.jpg" alt="Example checkout-flow instruction on the Viator site"/>
    <figcaption>Example checkout-flow instruction on the Viator site</figcaption>
</figure>

In this way, customers can be reassured that they are not being charged for a booking that may never be confirmed, thereby helping to reduce the number of calls to your customer service team.

### Combination purchases

If your customer has both instant and manual booking confirmation products in their shopping cart, only the amount for the confirmed booking(s) should be charged immediately; the portion corresponding to the pending bookings should be held as a pre-authorization against the customer’s credit card, and the transaction completed only once confirmation is received. 

It is important that you clearly differentiate between bookings that have been confirmed and those that are pending confirmation and communicate the status of each item. Also, be sure to make clear that the pre-authorization will only finalize once the manual confirmation bookings are confirmed.

### Confirmation page

Changes will need to be made to your confirmation page because it will not be possible for your customer to download their voucher immediately after completing a manual confirmation booking. Rather, the voucher will be made available after the booking is confirmed.

Vouchers for instant confirmation products, however, must be made available immediately following the completion of the booking process.

### Customer communications

Confirmation message (e.g., email) templates for manual confirmation bookings should indicate that the item is pending confirmation from the supplier; and, that confirmation for this activity will take up to 72 hours, depending on availability.

You will also need to create templates for the following scenarios:

- When the manual confirmation booking is confirmed by the supplier (this time including the voucher details)
- If the manual confirmation booking is rejected
- If multiple manual confirmation products have been booked:
    + When all items have been accepted/confirmed
    + When there are mixed statuses; i.e., 'pending' + 'rejected' + 'canceled'
    + If all items were rejected
- If a mixture of instant and manual confirmation products are booked at the same time; i.e., in the same cart

<u>**Note**</u>: When a booking is declined, it is useful to mention that the customer's card was not charged.

#### Example email for a manual confirmation booking pending confirmation:

<figure>
    <img src="https://docs.viator.com/partner-api/resources/merchant/technical/img/on-request-confirmation-email-1.jpg" alt="Viator website showing translation attribution"/>
    <img src="https://docs.viator.com/partner-api/resources/merchant/technical/img/on-request-confirmation-email-2.jpg" alt="Viator website showing translation attribution"/>
    <img src="https://docs.viator.com/partner-api/resources/merchant/technical/img/on-request-confirmation-email-3.jpg" alt="Viator website showing translation attribution"/>
</figure>

## Confirmation time-outs resulting in rejection

As mentioned above, when a booking request for a manual confirmation product is made, it will have a status of 'pending' until it is confirmed by the supplier. The supplier has the option of confirming or rejecting the booking.

If the supplier does not confirm the booking within 72 hours – or, if they have not confirmed the booking by the time it reaches 24 hours from the product's start time – our systems will automatically reject the booking, and this will be reflected in the response from the [/bookings/status](../../../openapi/reference/operation/bookingsStatus) endpoint.

## Building in the sandbox environment

Upgrading your booking platform to support manual confirmation products will require you to build and test this functionality in the sandbox environment. However, as no actual booking requests are made with the supplier when using a sandbox-only API-key, you will need to contact our API tech support team at [apitechsupport@viator.com](mailto:apitechsupport@viator.com) and request that the booking be confirmed or rejected as you require.

While necessary, please note that this is a manual process. We'd genuinely appreciate your effort in keeping the number of these requests to a minimum.