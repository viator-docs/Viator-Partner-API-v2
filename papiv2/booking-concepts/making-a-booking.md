# Making a booking

**Note**: This section applies to merchant partners only.

## Overview

Broadly speaking, to book a product via this API, you must do the following:

1. **Check availability**: Determine that a ticket for the desired tour or activity is available for a specific date, time and passenger mix using the [/availability/check](../../../openapi/reference/operation/availabilityCheck) endpoint.
2. **Request a booking hold**: If a ticket is availabile, request a pricing/availability hold for the booking using the [/bookings/hold](../../../openapi/reference/operation/bookingsHold) endpoint.
3. **Confirm the booking**: Confirm/finalize the held booking using the [/bookings/book](../../../openapi/reference/operation/bookingsBook) endpoint.

## 1. Check availability

A product's availability schedules indicate whether or not a product (including its product options) is scheduled to operate on a certain date and time. This information should be retrieved and kept up-to-date using any of the [availability schedules endpoints](../../key-concepts/content-ingestion-endpoints). 

However, as last-minute sales or availability changes by the supplier can result in a product that is scheduled to operate becoming unavailable immediately prior to booking, it is necessary to perform a final check to determine whether tickets remain available for sale or have been sold out. This is accomplished using the [/availability/check](../../../openapi/reference/operation/availabilityCheck) endpoint.

This endpoint should be called once the customer has selected a **date**, a **pax-mix** and one of the following:

| Criteria | [/availability/check](../../../openapi/reference/operation/availabilityCheck) response |
|----------|--------------------------------------------------------------|
| a **product** | availability for the product, product options and/or start times |
| a **product** and a **start time** | availability for the product at that start time |
| a **product** and a **product option** | availability for the product option or the product option start times, if applicable |
| a **product**, **product option** and a **start time** | availability for the product option's start time |

In order to keep processing fast, call the [/availability/check](../../../openapi/reference/operation/availabilityCheck) endpoint with as much information as you are able to specify; e.g., if you wish to display the availability for all product options and start times of a product, then only **date**, **pax-mix** and **product** are required. If you wish to check whether a specific start-time is available for a product, then the **start time** should also be included. 

**Note**: These are the only points in the workflow where the real-time availability check service should be used. For building calendars and other bulk operations, the [availability schedule endpoints]((../../key-concepts/content-ingestion-endpoints) should be used.

The information required to contruct a valid request for the availability check service is as follows:

| Request parameter | Source |
|-------------------|--------|
| `productCode` | `productCode` in response from in the response from any of the [product content endpoints](../../key-concepts/content-ingestion-endpoints) |
| `paxMix.ageBand` | One of the `ageBand` values (`"CHILD"`, `"INFANT"`, `"ADULT"` etc.) in the `pricingInfo.ageBands` array in the response from any of the [product content endpoints](../../key-concepts/content-ingestion-endpoints) |
| `paxMix.numberOfTravelers` | Selected by user |
| `currency` | Determined by partner |
| `productOptionCode` | If specified, the availability results will be restricted to this product-option code, one of `bookableItems[].productOptionCode` in the response from any of the [availability schedule endpoints](../../key-concepts/content-ingestion-endpoints). If omitted, availability information for all product options will be returned. See [Product options](../../key-concepts/product-options) for more information. |
| `startTime` | If specified, the availability results will be restricted to a certain starting time, one of `bookableItems[].seasons[].pricingRecords[].timedEntries[].startTime` in the response from any of the [availability schedule endpoints](../../key-concepts/content-ingestion-endpoints). | 
| `travelDate` | The desired date of travel that is an available date according to the response from any of the [availability schedule endpoints](../../key-concepts/content-ingestion-endpoints). See [Availability schedules](../../key-concepts/availability-schedules) for more information. |

### Example availability check for product [3857NYCNIA - 2-Day Niagara Falls and Tannersville Tour from New York by Bus](https://www.viator.com/tours/New-York-City/2-Day-Niagara-Falls-Tour-from-New-York-by-Bus/d687-3857NYCNIA)

**Example request to [/availability/check](../../../openapi/reference/operation/availabilityCheck)**:

```javascript
{
  "productCode": "3857NYCNIA",
  "productOptionCode": "NIA",
  "travelDate": "2021-04-01",
  "currency": "AUD",
  "startTime": "06:30",
  "paxMix": [
    {
      "ageBand": "ADULT",
      "numberOfTravelers": 2
    }
  ]
}
```

**Example response from [/availability/check](../../../openapi/reference/operation/availabilityCheck)**:

```javascript
{
    "currency": "AUD",
    "productCode": "3857NYCNIA",
    "travelDate": "2021-04-01",
    "bookableItems": [
        {
            "productOptionCode": "NIA",
            "startTime": "06:30",
            "available": true,
            "lineItems": [
                {
                    "ageBand": "ADULT",
                    "numberOfTravelers": 2,
                    "subtotalPrice": {
                        "price": {
                            "recommendedRetailPrice": 977.54,
                            "partnerNetPrice": 752.70
                        }
                    }
                }
            ],
            "totalPrice": {
                "price": {
                    "recommendedRetailPrice": 977.54,
                    "partnerNetPrice": 752.71,
                    "bookingFee": 45.16,
                    "partnerTotalPrice": 797.87
                }
            }
        }
    ]
}
```

The response above indicates that this combination of **product**, **product-option**, **date**, **start-time** and **pax-mix** IS available (`"available": true`). 

Note that the availability check only applies to the exact combination of the above. If any change is made to the **product**, **product-option**, **date**, **start-time** or **pax-mix**, it is necessary to call [/availability/check](../../../openapi/reference/operation//availabilityCheck) again with the updated information prior to requesting a booking hold. 

With this affirmative result, you can move onto the next step.

## 2. Request a booking hold

The request object that you will send to the [/bookings/hold](../../../openapi/reference/operation/bookingsHold) endpoint is identical to that of the request sent to the [/availability/check](../../../openapi/reference/operation/availabilityCheck) endpoint in the previous step:

**Example request to [/bookings/hold](../../../openapi/reference/operation/bookingsHold)**:

```javascript
{
  "productCode": "3857NYCNIA",
  "productOptionCode": "NIA",
  "travelDate": "2021-04-01",
  "currency": "AUD",
  "startTime": "06:30",
  "paxMix": [
    {
      "ageBand": "ADULT",
      "numberOfTravelers": 2
    }
  ]
}
```

**Example response from [/bookings/hold](../../../openapi/reference/operation/bookingsHold)**:

```javascript
{
    "bookingRef": "BR-787818552",
    "bookingHoldInfo": {
        "availability": {
            "status": "HOLD_NOT_PROVIDED"
        },
        "pricing": {
            "status": "HOLDING",
            "validUntil": "2020-09-15T03:23:27.529Z"
        }
    },
    "lineItems": [
        {
            "ageBand": "ADULT",
            "numberOfTravelers": 2,
            "subtotalPrice": {
                "price": {
                    "recommendedRetailPrice": 977.56,
                    "partnerNetPrice": 752.74
                }
            }
        }
    ],
    "totalPrice": {
        "price": {
            "recommendedRetailPrice": 977.56,
            "partnerNetPrice": 752.74,
            "bookingFee": 45.16,
            "partnerTotalPrice": 797.90
        }
    }
}
```

There are two components of the booking hold:
  - **Availability**: An availability hold ensures that the requested tickets will remain available for purchase.
  - **Pricing**: A pricing hold ensures that the requested tickets will remain at the prices quoted in the response.

The `bookingHoldInfo` object communicates the status and expiry of the pricing and availability holds.
  - `"HOLDING"` - the hold was successful
  - `"HOLD_NOT_PROVIDED"` - the hold was not successful

While an availability hold may not be granted, depending on the supplier's system, a pricing hold of 5 minutes will always be granted and is sufficient to progress to the next step, which is to finalize/confirm the booking using the [/bookings/book](../../../openapi/reference/operation/bookingsBook) endpoint.

There is no way to determine in advance of calling [/bookings/hold](../../../openapi/reference/operation/bookingsHold) whether an availability hold can or will be granted. Around 18% of the products in our catalogue do support this feature. Most products are confirmed instantly and do not rely on the concept of a limited inventory. The availability hold is for those products with a limited inventory that regularly sell out.

## 3. Confirm the booking

To finalize the booking for which you requested a booking-hold in the previous step, you will use the [/bookings/book](../../../openapi/reference/operation/bookingsBook) endpoint.

This service requires all relevant booking information to be provided in the request; i.e., extra information is required compared to that which you provided in the [/availability/check](../../../openapi/reference/operation//availabilityCheck) and [/bookings/hold](../../../openapi/reference/operation/bookingsHold) steps.

### Information required to confirm the booking

Compared to the request to the [/bookings/hold](../../../openapi/reference/operation/bookingsHold) endpoint, the additional information required is as follows:

| Element | Description |
|---------|-------------|
| `bookingRef` | This is the booking reference code that is generated by Viator and returned in the `bookingRef` element in the response from [/bookings/hold](../../../openapi/reference/operation/bookingsHold). |
| `partnerBookingRef` | A unique booking reference code for this booking generated by you to be compatible with your local system |
| `languageGuide` | You must specify one of the valid language guide options available for the product & product option to be booked. The language guides available for each product option are given in the `productOptions[].languageGuides[]` array in the response from any of the [product content endpoints](../../key-concepts/content-ingestion-endpoints). |
| `bookerInfo` | First and last names of the person making the booking |
| `bookingQuestionAnswers` | Answers to the [booking questions](../../booking-concepts/booking-questions) stipulated in the `bookingQuestions` array in the response from any of the [product content endpoints](../../key-concepts/content-ingestion-endpoints) |
| `communication` | Telephone number and email to be used for correspondence regarding the booking |
| `additionalBookingDetails` | Additional information to include with the booking; e.g., extra information to be included on the voucher |

**Request to [/bookings/book](../../../openapi/reference/operation/bookingsBook)**:

```javascript
{
  "productCode": "3857NYCNIA",
  "productOptionCode": "NIA",
  "currency": "AUD",
  "partnerBookingRef": "BR-787818552",
  "travelDate": "2021-04-01",
  "paxMix": [
    {
      "ageBand": "ADULT",
      "numberOfTravelers": 2
    }
  ],
  "languageGuide": {
    "type": "GUIDE",
    "language": "en"
  },
  "bookingRef": "BR-787818552",
  "bookerInfo": {
    "firstName": "Simon",
    "lastName": "Nettle"
  },
  "startTime": "06:30",
  "bookingQuestionAnswers": [  
    {
      "question": "AGEBAND",
      "answer": "ADULT",
      "travelerNum": 1
    },
    {
      "question": "FULL_NAMES_LAST",
      "answer": "Nettle",
      "travelerNum": 1
    },
    {
      "question": "FULL_NAMES_FIRST",
      "answer": "Simon",
      "travelerNum": 1
    },
    {
      "question": "AGEBAND",
      "answer": "ADULT",
      "travelerNum": 2
    },
    {
      "question": "FULL_NAMES_LAST",
      "answer": "Sim",
      "travelerNum": 2
    },
    {
      "question": "FULL_NAMES_FIRST",
      "answer": "Linda",
      "travelerNum": 2
    }
  ],
  "communication": {
    "email": "hsimpson@tripadvisor.com",
    "phone": "+61 123456789"
  },
  "additionalBookingDetails": {
    "voucherDetails": {
      "companyName": "Booking King",
      "email": "support@bookingking.com",
      "phone": "+61400500600",
      "voucherText": "For any enquiries, please visit our customer support page at https://support.bookingking.com"
    }
  }
}
```

**Response from [/bookings/book](../../../openapi/reference/operation/bookingsBook)**

```javascript
{
    "status": "CONFIRMED",
    "bookingRef": "BR-787818552",
    "partnerBookingRef": "BR-787818552",
    "currency": "AUD",
    "lineItems": [
        {
            "ageBand": "ADULT",
            "numberOfTravelers": 2,
            "subtotalPrice": {
                "price": {
                    "recommendedRetailPrice": 977.56,
                    "partnerNetPrice": 752.74
                }
            }
        }
    ],
    "totalPrice": {
        "price": {
            "recommendedRetailPrice": 977.56,
            "partnerNetPrice": 752.74,
            "bookingFee": 45.16,
            "partnerTotalPrice": 797.90
        }
    },
    "cancellationPolicy": {
        "type": "CUSTOM",
        "description": "If you cancel at least 6 day(s) before the scheduled departure time, you will receive a full refund.<br>If you cancel within 2 day(s) of the scheduled departure, you will receive a 0% refund.<br>If you cancel between 2 and 6 day(s) before the scheduled departure time, you will receive a 50% refund.",
        "cancelIfBadWeather": false,
        "cancelIfInsufficientTravelers": false,
        "refundEligibility": [
            {
                "dayRangeMin": 6,
                "percentageRefundable": 100,
                "startTimestamp": "2020-09-15T03:21:03.26406Z",
                "endTimestamp": "2021-03-26T10:30:00Z"
            },
            {
                "dayRangeMin": 0,
                "dayRangeMax": 2,
                "percentageRefundable": 0,
                "startTimestamp": "2021-03-30T10:30:00Z",
                "endTimestamp": "2021-04-01T10:30:00Z"
            },
            {
                "dayRangeMin": 2,
                "dayRangeMax": 6,
                "percentageRefundable": 50,
                "startTimestamp": "2021-03-26T10:30:00Z",
                "endTimestamp": "2021-03-30T10:30:00Z"
            }
        ]
    },
    "voucherInfo": {
        "url": "https://www.viator.com/ticket?code=1183891096:14dfa2898750a3a7753da82cd447be6e172aa59c392d5bfd2d21b17b1ce98d0e",
        "format": "HTML"
    }
}
```

This response (`"status": "CONFIRMED"`) indicates that the booking was successful.