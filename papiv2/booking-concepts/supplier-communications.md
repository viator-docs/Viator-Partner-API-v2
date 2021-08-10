## Supplier communications

**Note**: This section applies to merchant partners only.

### How can suppliers communicate with end customers?

Suppliers occasionally need to reach out to customers for a variety of reasons, such as:

* Requesting pick-up locations, flight details or passenger weight information
* Providing weather alerts, sold-out notifications or general messaging

To allow suppliers to contact customers directly, Viator provides **Closed-Loop Communication (CLC)**.

### How to set up CLC for a booking

The recipient(s) of suppliers' CLC messages is set for each booking at the time of booking by supplying the customer’s email address (`email`) as well as their phone number (`phone`) in the `communication` object in the request sent to the [/booking/book](../../../openapi/reference/operation/bookingsBook) service when making a booking.

This will direct CLC messages sent by suppliers directly to that customer's email; no action from your support team will be necessary for suppliers to communicate with customers.

Merchants choosing this option should mention to their customers that they are purchasing a product from a third-party supplier, and that they may, therefore, receive communications regarding the purchase directly from that supplier.

#### Note:

* To have a CC of each message a supplier sends to their customer sent to *your* (the partner's) customer support email address (in case further assistance is required) you must include your customer support email address in the `email` field, after the customer's email and separated from that address with a comma.


#### Example request body snippet to enable direct CLC
```javascript
{
    ...
    "communication": {
      "email": "snettle@tripadvisor.com",
      "phone": "+61 4121121121"
}
```

#### Example request body snippet to enable direct CLC with a CC of each message sent to your customer support
```javascript
{
    ...
    "communication": {
      "email": "snettle@tripadvisor.com,customersupport@bookatrip4me.com",
      "phone": "+61 4121121121"
}
```

### Supplier communications without CLC

To have CLCs from the supplier sent <u>only</u> to your (the merchant's) customer support team, supply the email address of your customer support function in the `email` field of the `communication` object in the request to the [/bookings/book](../../../openapi/reference/operation/bookingsBook) service when making a booking.

**Note:** Utilizing this option requires you, the merchant, to manage the final loop of communication with the end customer to ensure that their tour/activity can be fulfilled successfully.

#### Example request body snippet to disable CLC
```javascript
{
    ...
    "communication": {
      "email": "customersupport@bookatrip4me.com",
      "phone": "+61 4121121121"
}
```

### Default behavior

If an email address is not supplied in the communications object, the default behavior will be to use the partner's customer-support email address for correspondence regarding this booking.

#### Example request body snippet that would trigger the default behavior

```javascript
{
    ...
    "communication": {
      "phone": "+61 4121121121"
}
```
