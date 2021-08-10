# Ingesting and updating availability schedules

Availability schedule information for all products is available to be ingested and updated via the [/availablity/schedules/modified-since](../../../openapi/reference/operation/availabilitySchedulesModifiedSince) endpoint. This endpoint functions in a similar manner to the [/products/modified-since](../../../openapi/reference/operation/productsModifiedSince) endpoint; i.e., an initial call is made that returns all availability data in bulk from the beginning of time until the present, and then calls are made periodically to that same endpoint, which will return a delta update.

Therefore, to initialize your local copy of our availability schedule information, make a call to [/availability/schedules/modified-since](../../../openapi/reference/operation/availabilitySchedulesModifiedSince), including only the `count` query parameter, which we recommend setting to `500`. 

For the sake of brevity, in the following example, `count` is set to `5`

**Example request**

```
GET https://api.viator.com/partner/availability/schedules/modified-since?count=5
```

You will receive a response similar to the following:

```javascript
{
    "availabilitySchedules": [
        {
            "productCode": "3320STOLZadvaft",
            "bookableItems": [
                {
                    "seasons": [
                        {
                            "startDate": "2020-11-23",
                            "endDate": "2020-12-22",
                            "pricingRecords": [
                                {
                                    "daysOfWeek": [
                                        "WEDNESDAY",
                                        "THURSDAY",
                                        "FRIDAY",
                                        "SATURDAY",
                                        "SUNDAY"
                                    ],
                                    "timedEntries": [
                                        {
                                            "startTime": "15:30"
                                        }
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
                                                    "recommendedRetailPrice": 17.20,
                                                    "partnerNetPrice": 11.50,
                                                    "bookingFee": 0.69,
                                                    "partnerTotalPrice": 12.19
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
            "currency": "EUR"
        },
        {
            "productCode": "14093P1",
            "bookableItems": [],
            "currency": "EUR"
        },
        {
            "productCode": "3567AKL_SAF",
            "bookableItems": [],
            "currency": "NZD"
        },
        {
            "productCode": "5855RIPLEYS",
            "bookableItems": [],
            "currency": "USD"
        },
        {
            "productCode": "3033ENTRY_TR",
            "bookableItems": [],
            "currency": "USD"
        }
    ],
    "nextCursor": "MTU3ODA2Njc4NnwzMDMzRU5UUllfVFI="
}
```

The endpoint returns an array (`availabilitySchedules`) where each item contains all availabilty schedule information for a single product, and a cursor (`nextCursor`) that is to be used in subsequent calls to this endpoint to retrieve the next page of results.

For details on how to interpret the availability schedule object, see [Availability schedules](#section/key-concepts/availability-schedules).

**Note**: An empty `bookableItems` array means that the product indicated by `productCode` is not active, has no availability, and therefore cannot be booked. This fact will be reflected if the product details are requested from the [/products/{product-code}](../../../openapi/reference/operation/products) endpoint; e.g., for product `3033ENTRY_TR`:

```javascript
{
    "status": "INACTIVE",
    "productCode": "3033ENTRY_TR",
    "language": "en",
    "createdAt": "2006-05-01T07:00:00Z"
}
```

## Pagination

As with the [/products/modified-since](../../../openapi/reference/operation/productsModifiedSince) endpoint, you will receive as many records as requested via the `count` request parameter. Included in the response is the `nextCursor` element, which contains a code that points to the next page of availability records.

After receiving this first response, your next request to the [/availablity/schedules/modified-since](../../../openapi/reference/operation/availabilitySchedulesModifiedSince) service should include the value of `nextCursor` in the `cursor` request parameter; i.e.:

```
GET https://api.sandbox.viator.com/partner/availability/schedules/modified-since?count=5&cursor=MTU3ODA2Njc4NnwzMDMzRU5UUllfVFI=
```

The response will be similar to the initial response shown above, except it will contain the next `count` number of availability schedule records and a new `nextCursor` that points to the following page.

Loop through this process until you receive a response that contains an empty `availabilitySchedules` array and <u>does not</u> contain a `nextCursor` element. The absence of the `nextCursor` element indicates that you have, for the time being, reached the end of the list and your availability information is fully up-to-date.

**Example final page response**

```javascript
{
    "availabilitySchedules": []
}
```

## Periodically update your availability information

Once your availability schedule information ingestion is complete, you can keep it up-to-date by periodically polling the service using the latest `nextCursor` code you received; i.e., from the page prior to the final, empty page. 

If new availability schedule information is available, the service will respond with new availability information in the `availabilitySchedules` array and a new `nextCursor` element with which to poll the service for future updates in the same way. Note that the `nextCursor` code is valid indefinitely; it will not expire.

As with the [/products/modified-since](../../../openapi/reference/operation/productsModifiedSince) endpoint, we recommend polling this service on an hourly basis. The longer the interval between updates, the more likely your availability information will be out of date, raising the likelihood of availability differences when you make a real-time availability check using the [/availability/check](../../../openapi/reference/operation/availabilityCheck) service.

Again, you should only need to call [/availablity/schedules/modified-since](../../../openapi/reference/operation/availabilitySchedulesModifiedSince) without a `cursor` parameter **once**, for the first call of the initial ingestion. All future calls should include the `cursor` parameter and the results used to update your database.

## Rate limiting

We apply a rate limit of 150 requests per 10 second time window (rolling) for all requests to this API.

Request rates exceeding this limit will result in a HTTP 429 (Too Many Requests) status code being returned.

In order to avoid running-up against the rate limits, insert a delay of 2s if you receive a HTTP 429 status code. Furthermore, do not perform this operation as a multi-threaded process.

## Filtering out availability schedules

As you may not be offering Viator's full product catalogue for sale on your site, you are only required to store availability information for the products you support. Therefore, if you are filtering out products from our catalogue, you should also perform a check with regard to the availability schedule information received from the [/availablity/schedules/modified-since](../../../openapi/reference/operation/availabilitySchedulesModifiedSince) to ensure that it pertains to a product in your catalogue.

This can be done by checking the `productCode` field in the ProductAvailabilitySchedule object response.