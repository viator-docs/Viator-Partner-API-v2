# Cancellation API workflow

**Note**: This section applies to merchant partners only.

All booking cancellations (except for those requested after the date of travel) must now be performed via the API. Viator no longer offers ordinary cancellation services via our customer support function. To cancel a booking after the tour or activity has occurred, please contact [Viator Partner Support](mailto:dpsupport@viator.com) 

## Getting cancellation reasons
<a id="getting-cancellation-reasons"></a>

When canceling a booking, you are required to submit a valid 'reason for the cancellation' to assist with Viator's internal processes. This is accomplished via the inclusion of a valid reason code in the body of the request. The reason codes can be retrieved from the [/bookings/cancel-reasons](../../../openapi/reference/operation/bookingsCancelReasons) endpoint.

As the acceptable reasons for cancellation may be altered at any point, we recommend retrieving an up-to-date list from this endpoint at least weekly.

The output from the [/bookings/cancel-reasons](#operation/bookingsCancelReasons) endpoint at the time of writing is as follows:

```javascript
{
    "reasons": [
        {
            "cancellationReasonText": "Supplier No Show",
            "cancellationReasonCode": "Customer_Service.Supplier_no_show"
        },
        {
            "cancellationReasonText": "Chose a different/cheaper tour",
            "cancellationReasonCode": "Customer_Service.Chose_a_different_cheaper_tour"
        },
        {
            "cancellationReasonText": "Unexpected medical circumstances",
            "cancellationReasonCode": "Customer_Service.Unexpected_medical_circumstances"
        },
        {
            "cancellationReasonText": "Weather",
            "cancellationReasonCode": "Customer_Service.Weather"
        },
        {
            "cancellationReasonText": "Booked wrong tour date",
            "cancellationReasonCode": "Customer_Service.Booked_wrong_tour_date"
        },
        {
            "cancellationReasonText": "Duplicate Booking",
            "cancellationReasonCode": "Customer_Service.Duplicate_Booking"
        },
        {
            "cancellationReasonText": "Significant global event",
            "cancellationReasonCode": "Customer_Service.Significant_global_event"
        },
        {
            "cancellationReasonText": "I canceled my entire trip",
            "cancellationReasonCode": "Customer_Service.I_canceled_my_entire_trip"
        }
    ]
}
```

## Canceling a booking

### Getting a cancellation quote

Before canceling the booking, call the [/bookings/{booking-reference}/cancel-quote](../../../openapi/reference/operation/bookingsCancelQuote) endpoint to get information about whether the booking can be canceled using this endpoint and what the refund will be, for example:

```
GET https://api.viator.com/partner/bookings/BR-580254558/cancel-quote
```

- **Note**: For bookings made with v1 of this API, this code corresponds to `data.itemSummaries[].itemId` (in the response from v1's [/booking/book](https://docs.viator.com/partner-api/merchant/technical/#tag/Booking-services/paths/~1booking~1book/post) endpoint) but prepended with `BR-`. For example, if the `itemId` is `580254558`, this field should be `BR-580254558`.

You will receive a cancellation quote object, e.g.:

```javascript
{
    "bookingId": "BR-580254558",
    "status": "CANCELLABLE",
    "refundDetails": {
        "itemPrice": 109.77,
        "refundAmount": 109.77,
        "currencyCode": "USD",
        "refundPercentage": 100.00
    }
}
```

**Note**: Bookings that have not been confirmed by the supplier and have a status of `"PENDING"` will report an `itemPrice`, `refundAmount` and `refundPercentage` of `0`, as no fees are charged until the booking's status is `"CONFIRMED"`.
 
The data elements in this object have meanings as follows:

| Element | Meaning | Example |
|---------|---------|---------|
| `bookingId` | the booking reference number prepended with `BR-` | `BR-580254556` |
| `status` | One of the following: <ul><li>`CANCELLABLE`: the booking is eligible to be cancelled</li><li>`CANCELLED`: the booking has already been cancelled</li><li>`NOT_CANCELLABLE`: the booking is for a product that operated in the past, and therefore cannot be cancelled using this endpoint (you will need to [send an email to dpsupport](mailto:dpsupport@viator.com) including both "CANCEL" and the booking reference number in the subject line in order to request a refund for such a booking)</li></ul> | `CANCELLABLE` |
| `refundDetails` | object containing information about the refund | |
| `itemPrice` | the **merchant net price** + **transaction fee** for this product at the time of booking in the currency specified by `currencyCode` | `109.77` |
| `refundAmount` | the amount that will be deducted from your invoice if the booking is cancelled now | `109.77` |
| `currencyCode` | the currency code for the currency in which pricing information is displayed | `USD` |
| `refundPercentage` | the refund amount expressed as a percentage of the `itemPrice` | `100.00` |


**Will there ever be a discrepancy between the amount charged for a tour and the amount refunded due to currency exchange-rate fluctuations?**

- In short: no. Firstly, the cost of the booking and the refund amount are always calculated in the product supplier's native currency – no exchange rate calculations are applied. I.e., if the cost of the booking was USD 100 and the refund percentage is 100% (full refund, as per the response from [/bookings/{booking-reference}/cancel-quote](../../../openapi/reference/operation/bookingsCancelQuote)), Viator will simply not invoice you for that USD 100 that we would have if the booking was not canceled. Furthermore, we do not invoice you for the cost of a booking prior to its departure date.

### Requesting the cancellation

If the `status` field has a value of `CANCELLABLE` and you are happy with the `refundAmount`, call the [/bookings/{booking-reference}/cancel](../../../openapi/reference/operation/bookingsCancel) endpoint to cancel the booking, e.g.:

```
POST https://api.viator.com/partner/bookings/BR-580254558/cancel
```

A reason code corresponding to the reason for cancellation must be included in the request body; e.g.:

```javascript
{
  "reasonCode":"Customer_Service.Chose_a_different_cheaper_tour"
}
```

You should receive a response indicating that the cancellation was successful, e.g.:

```javascript
{
    "bookingId": "BR-580254558",
    "status": "ACCEPTED"
}
```

A `status` of `ACCEPTED` indicates that the booking was successfully canceled.