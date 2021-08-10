# Booking cutoff times

**Note**: This section applies to merchant partners only.

This section describes how thebooking cutoff time information given by the product supplier, which may affect a product's availability and therefore its ability to be booked, is communicated via this API. While you are welcome to develop logic to support the display and utilisation of this information, **it is not necessary to do so**. Indeed, as most implementations are unlikely to benefit, we recommend simply using the real-time [/availability/check](../../../openapi/reference/operation/availabilityCheck) endpoint as the primary means to determine and communicate a product's availability in your product booking workflow.

Many products in our inventory are subject to a booking cutoff time. Tickets for such products may only be purchased up until a certain time. This information is given in the `bookingConfirmationSettings` object in the [product content response](../../key-concepts/content-ingestion-endpoints).

Example `bookingConfirmationSettings` object:

```javascript
"bookingConfirmationSettings": {
  "bookingCutoffType": "START_TIME",
  "bookingCutoffInMinutes": 0,
  "confirmationType": "INSTANT"
},
```

There are four booking cutoff types, indicated by the value of the `bookingCutoffType` element; they are:
  - `"START_TIME"` – booking cutoff is relative to the product's start time
  - `"OPENING_TIME"` – booking cutoff is relative to the product's opening time
  - `"CLOSING_TIME"` – booking cutoff is relative to the product's closing time
  - `"FIXED_TIME"` – booking cutoff is relative to the time given in the `bookingCutoffFixedTime` element

In addition, the booking cutoff time may be offset by the number of minutes given in `bookingCutoffInMinutes`; for example, product 57377P9's booking cutoff is 120 minutes prior to its start time:

```javascript
"bookingConfirmationSettings": {
  "bookingCutoffType": "START_TIME",
  "bookingCutoffInMinutes": 120,
  "confirmationType": "INSTANT"
}
```

The availability of products that have a `bookingCutoffType` of `"START_TIME"` can be determined by inspecting the `pricingRecords[].timedEntries[].startTime` field in the response from [/availability/schedules/{product-code}](../../../openapi/reference/operation/availabilitySchedules). This type of product can be booked up until `bookingCutoffInMinutes` prior to its starting time in the time-zone in which the product operates, which is given in the `timeZone` field in the [product content response](../../key-concepts/content-ingestion-endpoints).

- **Note**: At present, we do not communicate a product's opening or closing times via the API. Therefore, in order to determine the availability of such products, you will need to call the [/availability/check](../../../openapi/reference/operation/availabilityCheck) endpoint. This information will be made available through this API in future.

The booking cutoff time for products with a `"FIXED_TIME"` type is that given in the `bookingCutoffFixedTime` element, offset by the number of minutes given in `bookingCutoffInMinutes`. In the fixed time scenario, `bookingCutoffInMinutes` can only be 0, 1440 (24 hours) or 2880 (48 hours). This means that the booking cutoff time is at a certain time on the day on which the product operates; or, at that same time but one or two days before the tour date, respectively.

In the following example, the booking cutoff is 10:00am on the day before the tour or activity operates:

```javascript
"bookingConfirmationSettings": {
    "bookingCutoffType": "FIXED_TIME",
    "bookingCutoffInMinutes": 1440,
    "confirmationType": "INSTANT",
    "bookingCutoffFixedTime": "10:00:00"
}
```

You may not be able to book a product (using [/bookings/book](../../../openapi/reference/operation/bookingsBook)) or place a booking hold on a product using [/bookings/hold](../../../openapi/reference/operation/bookingsHold) after it is past the booking cutoff time. However, this is not always the case. Always check real-time availability using the [/availability/check](../../../openapi/reference/operation/availabilityCheck) endpoint.

Nonetheless, in displaying the availability of a product on your site, you may wish to mark products as 'unavailable' for a particular day or starting time if it is presently past the booking cutoff time if you feel that doing so will improve your site's UX.

However, because the availability schedule data in general can rapidly fall out of date, **we again encourage you to always utilise the real-time [/availability/check](../../../openapi/reference/operation/availabilityCheck) endpoint as the primary means to determine and communicate a product's availability in your product booking workflow.**