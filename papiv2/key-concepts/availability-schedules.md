# Availability schedules

The [availability schedules endpoints](../content-ingestion-endpoints/#availability-schedules-endpoints) provide information about the availability of a product (along with its various product options and starting times, if they exist) and its pricing.

This section explains how to interpret the response from the [availability schedules endpoints](../content-ingestion-endpoints/#availability-schedules-endpoints).

Example response snippet from [/availability/schedules/10212P2](../../../openapi/reference/operation/availabilitySchedules/#availability-schedules-endpoints) (some starting times were removed for brevity):


```javascript
{
    "productCode": "10212P2",
    "bookableItems": [
        {
            "productOptionCode": "TG45",
            "seasons": [
                {
                    "startDate": "2019-05-01",
                    "endDate": "2021-12-31",
                    "pricingRecords": [
                        {
                            "daysOfWeek": [
                                "MONDAY",
                                "TUESDAY",
                                "WEDNESDAY",
                                "THURSDAY",
                                "FRIDAY",
                                "SATURDAY",
                                "SUNDAY"
                            ],
                            "timedEntries": [
                                {
                                    "startTime": "11:00",
                                    "unavailableDates": [
                                        {
                                            "date": "2020-09-16",
                                            "reason": "SOLD_OUT"
                                        },
                                        {
                                            "date": "2020-09-23",
                                            "reason": "SOLD_OUT"
                                        },
                                        {
                                            "date": "2020-09-22",
                                            "reason": "SOLD_OUT"
                                        },
                                        {
                                            "date": "2020-09-27",
                                            "reason": "SOLD_OUT"
                                        },
                                        {
                                            "date": "2020-09-29",
                                            "reason": "SOLD_OUT"
                                        },
                                        {
                                            "date": "2020-09-30",
                                            "reason": "SOLD_OUT"
                                        }
                                    ]
                                },
                                {
                                    "startTime": "10:00",
                                    "unavailableDates": [
                                        {
                                            "date": "2020-09-25",
                                            "reason": "SOLD_OUT"
                                        },
                                        {
                                            "date": "2020-09-26",
                                            "reason": "SOLD_OUT"
                                        },
                                        {
                                            "date": "2020-09-28",
                                            "reason": "SOLD_OUT"
                                        },
                                        {
                                            "date": "2020-09-27",
                                            "reason": "SOLD_OUT"
                                        },
                                        {
                                            "date": "2020-09-30",
                                            "reason": "SOLD_OUT"
                                        },
                                        {
                                            "date": "2020-09-29",
                                            "reason": "SOLD_OUT"
                                        }
                                    ]
                                },
                                {...}
                            ],
                            "pricingDetails": [
                                {
                                    "pricingPackageType": "PER_PERSON",
                                    "minTravelers": 1,
                                    "ageBand": "INFANT",
                                    "price": {
                                        "original": {
                                            "recommendedRetailPrice": 0.00,
                                            "partnerNetPrice": 0.00,
                                            "bookingFee": 0.00,
                                            "partnerTotalPrice": 0.00
                                        }
                                    }
                                },
                                {
                                    "pricingPackageType": "PER_PERSON",
                                    "minTravelers": 1,
                                    "ageBand": "ADULT",
                                    "price": {
                                        "original": {
                                            "recommendedRetailPrice": 100.02,
                                            "partnerNetPrice": 79.89,
                                            "bookingFee": 4.79,
                                            "partnerTotalPrice": 84.68
                                        },
                                        "special": {
                                            "recommendedRetailPrice": 90.02,
                                            "partnerNetPrice": 71.90,
                                            "bookingFee": 4.31,
                                            "partnerTotalPrice": 76.21,
                                            "offerStartDate": "2020-08-28",
                                            "offerEndDate": "2020-09-30"
                                        }
                                    }
                                }
                            ]
                        }
                    ]
                }
            ]
        }
    ],
    "currency": "USD",
    "summary": {
        "fromPrice": 100.02
    }
}
```

- Each item in the `bookableItems[]` array contains the availability and pricing data for one of the product's product-options (`productOptionCode`).
- Each item in the `seasons[]` array describes the availability and pricing for the product-option during the 'season' (period of time) delimited by `startDate` and `endDate`. If `endDate` is not present, this means the season extends 384 days (approximately 12.5 months) into the future from the current date.
- Each item in the `pricingRecords[]` array describes which days of the week (`daysOfWeek[]`) the availability and pricing data pertains to during the season, as well as which starting times are in operation (`timedEntries[]`) and on which dates (`unavailableDates[]`) each starting time is unavailable (e.g., due to being sold out).
- Each item in the `pricingDetails[]` array describes the pricing that applies to each age-band during the season.

## Interpreting the availability schedule

We can thereby interpret the availability schedule snippet above as follows:

- For the product-option code `"TG45"` of product `"10212P2"`, during the period from `"2019-05-01"` to `"2021-12-31"`, on **all days of the week**, there are starting times at **10:00** and **11:00**.
- The **11:00** starting time is unavailable due to being 'sold out' on the following dates:
  - 2020-09-16
  - 2020-09-22
  - 2020-09-23
  - 2020-09-27
  - 2020-09-29
  - 2020-09-30
- The **10:00** starting time is unavailable due to being 'sold out' on the following dates:
  - 2020-09-25
  - 2020-09-26
  - 2020-09-27
  - 2020-09-28
  - 2020-09-29
  - 2020-09-30
- Infant tickets are free (USD 0)
- Adults are charged per-person; the normal recommended retail price (`original`) is USD 100.02
- A **pricing special** (`special`) is in effect for adults between 2020-08-28 and 2020-09-30; the special recommended retail price is USD 90.02 

Using this information, a complete schedule – including which product-codes and starting times are available on which dates and the pricing (including special discounts) that applies on each date - can be constructed in your database.