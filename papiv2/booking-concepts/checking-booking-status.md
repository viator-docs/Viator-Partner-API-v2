# Checking booking status

**Note**: This section applies to merchant partners only.

Once a booking has been made, you can check on its status using the [/bookings/status](../../../openapi/reference/operation/bookingsStatus) endpoint.

You can use **either** the `bookingRef` **or** `partnerBookingRef` in the request to this service.

- If the booking was made using v1 of the Viator Partner API, you will receive a HTTP 400 Bad Request response. Only bookings made using v2 or later of this API are compatible with this endpoint.
- If the booking is the subject of a booking hold (via [/bookings/hold](../../../openapi/reference/operation/bookingsHold)) but the booking has not yet been confirmed using [/bookings/book](../../../openapi/reference/operation/bookingsBook), you will receive a HTTP 404 Not Found response.
- If the booking was confirmed but later canceled, you will receive a HTTP 200 Success response that indicates the canceled status.
- If the booking has been confirmed and has not been canceled, you will receive a response with the same structure as the response from [/bookings/book](../../../openapi/reference/operation/bookingsBook)

## Confirmed booking

**Example request**: Get the status of the booking with the booking reference `"BR-791143912"`

```javascript
{
  "bookingRef": "BR-791143912"
}
```

**Example response**: The status of this booking is `"CONFIRMED"`

```javascript
{
  "status": "CONFIRMED",
  "bookingRef": "BR-791143912",
  "partnerBookingRef": "BR-791143912",
  "currency": "USD",
  "lineItems": [
    {
      "ageBand": "ADULT",
      "numberOfTravelers": 2,
      "subtotalPrice": {
        "price": {
          "recommendedRetailPrice": 21.4,
          "partnerNetPrice": 20.52
        }
      }
    }
  ],
  "totalPrice": {
    "price": {
      "recommendedRetailPrice": 21.4,
      "partnerNetPrice": 20.52,
      "bookingFee": 1.23,
      "partnerTotalPrice": 21.75
    }
  },
  "cancellationPolicy": {
    "type": "STANDARD",
    "description": "For a full refund, cancel at least 24 hours before the scheduled departure time.",
    "cancelIfBadWeather": false,
    "cancelIfInsufficientTravelers": false,
    "refundEligibility": [
      {
        "dayRangeMin": 2,
        "percentageRefundable": 100,
        "startTimestamp": "2020-11-10T02:04:15Z",
        "endTimestamp": "2021-03-30T23:00:00Z"
      },
      {
        "dayRangeMin": 0,
        "dayRangeMax": 1,
        "percentageRefundable": 0,
        "startTimestamp": "2021-03-31T23:00:00Z",
        "endTimestamp": "2021-04-01T23:00:00Z"
      }
    ]
  },
  "voucherInfo": {
    "url": "https://www.viator.com/ticket?code=1187186744:8ac3e6308555ab632fe89e8a6b83c41620e840a3bc485f6e0d836192eae1ffc4",
    "format": "HTML"
  }
}
```


## Canceled booking

**Example request**: Get the status of the booking with the partner booking reference `"test-partner-ref-aejsmont-num-1604985575"`

```javascript
{
  "partnerBookingRef": "test-partner-ref-aejsmont-num-1604985575"
}
```

**Example response**: The status of this booking is `"CANCELED"`

```javascript
{
  "bookingRef": "BR-582323272",
  "status": "CANCELED",
  "partnerBookingRef": "test-partner-ref-aejsmont-num-1604985575"
}
```