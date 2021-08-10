# Ingesting and updating the product catalogue

The recommended way to initialize and keep your local copy of our products database up-to-date is by using the [/products/modified-since](../../../openapi/reference/operation/productsModifiedSince) endpoint in the following way:

## Initialize your local copy of our products database by ingesting all product records

Make a request to [/products/modified-since](../../../openapi/reference/operation/productsModifiedSince), but only include the `count` parameter. Do not supply values for `modified-since` or `cursor`. This instructs the system to return any products modified since the beginning of time; i.e., all of them.

**Note**: This is the <u>only</u> occasion on which you will need to call [/products/modified-since](../../../openapi/reference/operation/productsModifiedSince) without the `modified-since` or `cursor` parameter.

`count` specifies how many product records will be returned per page. For practical purposes, setting `count` to its maximum value (500) is advised. However, for the purposes of brevity, I am using a `count` of `5` in this example; i.e.:

```
GET https://api.sandbox.viator.com/partner/products/modified-since?count=5
```

You will receive a response similar to the following:

```javascript
{
    "products": [
        {
            "status": "ACTIVE",
            "productCode": "92457P4",
            "language": "en-US",
            "createdAt": "2020-06-16T12:49:29.519174Z",
            "lastUpdatedAt": "2020-11-08T16:26:16Z",
            "title": "Ecuador Parapente Quito",
            "ticketInfo": {
                "ticketTypes": [
                    "MOBILE_ONLY"
                ],
                "ticketTypeDescription": "Mobile or paper ticket accepted",
                "ticketsPerBooking": "ONE_PER_BOOKING",
                "ticketsPerBookingDescription": "One per booking"
            },
            "pricingInfo": {
                "type": "PER_PERSON",
                "ageBands": [
                    {
                        "ageBand": "INFANT",
                        "startAge": 6,
                        "endAge": 17,
                        "minTravelersPerBooking": 0,
                        "maxTravelersPerBooking": 4
                    },
                    {
                        "ageBand": "ADULT",
                        "startAge": 18,
                        "endAge": 60,
                        "minTravelersPerBooking": 0,
                        "maxTravelersPerBooking": 4
                    }
                ]
            },
            "images": [
                {
                    "imageSource": "SUPPLIER_PROVIDED",
                    "caption": "",
                    "isCover": true,
                    "variants": [...
                    ],
            "logistics": {
                "start": [
                    {
                        "location": {
                            "ref": "LOC-o0AXGEKPN4wJ9sIG0RAn5Cdd0Y9TkTxkcosDq0rJgjR12IzpogNi5POX+yGLXEoq"
                        },
                        "description": "The meeting point is on the way to Lumbisí km 1 at the entrance to La Primavera at 8:30 am."
                    }
                ],
                "end": [
                    {
                        "location": {
                            "ref": "LOC-o0AXGEKPN4wJ9sIG0RAn5Cdd0Y9TkTxkcosDq0rJgjR12IzpogNi5POX+yGLXEoq"
                        },
                        "description": "The meeting point is on the way to Lumbisí km 1 at the entrance to La Primavera at 8:30 am."
                    }
                ],
                "redemption": {
                    "redemptionType": "NONE",
                    "specialInstructions": ""
                },
                "travelerPickup": {
                    "pickupOptionType": "MEET_EVERYONE_AT_START_POINT",
                    "allowCustomTravelerPickup": true
                }
            },
            "timeZone": "America/Guayaquil",
            "description": "the only legal paragliding operator in ecuador. See an amazing view of Quito and its surroundings from above.",
            "inclusions": [
                {
                    "category": "OTHER",
                    "categoryDescription": "Other",
                    "type": "OTHER",
                    "typeDescription": "Other",
                    "otherDescription": "Paragliding Kit"
                },
                {
                    "category": "OTHER",
                    "categoryDescription": "Other",
                    "type": "OTHER",
                    "typeDescription": "Other",
                    "otherDescription": "Air-conditioned vehicle"
                }
            ],
            "additionalInfo": [
                {
                    "type": "STROLLER_ACCESSIBLE",
                    "description": "Infants and small children can ride in a pram or stroller"
                },
                {
                    "type": "NO_PREGNANT",
                    "description": "Not recommended for pregnant travelers"
                },
                {
                    "type": "NO_HEART_PROBLEMS",
                    "description": "Not recommended for travelers with poor cardiovascular health"
                },
                {
                    "type": "PHYSICAL_EASY",
                    "description": "Suitable for all physical fitness levels"
                }
            ],
            "cancellationPolicy": {
                "type": "STANDARD",
                "description": "For a full refund, cancel at least 24 hours before the scheduled departure time.",
                "cancelIfBadWeather": true,
                "cancelIfInsufficientTravelers": false,
                "refundEligibility": [
                    {
                        "dayRangeMin": 1,
                        "percentageRefundable": 100
                    },
                    {
                        "dayRangeMin": 0,
                        "dayRangeMax": 1,
                        "percentageRefundable": 0
                    }
                ]
            },
            "bookingConfirmationSettings": {
                "bookingCutoffType": "FIXED_TIME",
                "bookingCutoffInMinutes": 1440,
                "confirmationType": "INSTANT",
                "bookingCutoffFixedTime": "16:00:00"
            },
            "bookingRequirements": {
                "minTravelersPerBooking": 1,
                "maxTravelersPerBooking": 4,
                "requiresAdultForBooking": false
            },
            "bookingQuestions": [
                "FULL_NAMES_LAST",
                "PASSPORT_EXPIRY",
                "PASSPORT_PASSPORT_NO",
                "SPECIAL_REQUIREMENTS",
                "FULL_NAMES_FIRST",
                "WEIGHT",
                "AGEBAND",
                "HEIGHT",
                "PASSPORT_NATIONALITY"
            ],
            "tags": [
                20234
            ],
            "destinations": [
                {
                    "ref": "735",
                    "primary": true
                }
            ],
            "itinerary": {
                "itineraryType": "ACTIVITY",
                "skipTheLine": false,
                "privateTour": true,
                "duration": {
                    "fixedDurationInMinutes": 15
                },
                "activityInfo": {
                    "location": {
                        "ref": "LOC-o0AXGEKPN4wJ9sIG0RAn5Cdd0Y9TkTxkcosDq0rJgjR12IzpogNi5POX+yGLXEoq"
                    },
                    "description": "We take off from the hill of Lumbisi towards the City, we observe the landscape of the city of Quito you can observe the mountains. We landed at the foot of the hill.\n\nThe meeting point is on the way to Lumbisí km 1 at the entrance to La Primavera at 8:30 am. From there we got on a vehicle to the mountain. "
                }
            },
            "productOptions": [
                {
                    "productOptionCode": "TG1",
                    "description": "",
                    "title": "Ecuador Parapente Quito"
                }
            ],
            "translationInfo": {
                "containsMachineTranslatedText": false
            },
            "supplier": {
                "name": "Opeturmo - Parapente Montanita"
            }
        },
        {
            "status": "INACTIVE",
            "productCode": "141567P21",
            "language": "en-US",
            "createdAt": "2019-10-08T13:12:21.535614Z",
            "lastUpdatedAt": "2020-11-13T13:41:18Z"
        },
        {
            "status": "INACTIVE",
            "productCode": "8932P17",
            "language": "en-US",
            "createdAt": "2019-06-03T13:29:09.131851Z",
            "lastUpdatedAt": "2020-11-13T13:42:38Z"
        },
        {
            "status": "INACTIVE",
            "productCode": "58593P53",
            "language": "en-US",
            "createdAt": "2020-11-13T13:46:39.927509Z",
            "lastUpdatedAt": "2020-11-13T13:51:43Z"
        },
        {
            "status": "INACTIVE",
            "productCode": "164413P1",
            "language": "en-US",
            "createdAt": "2019-06-14T00:29:29.039245Z",
            "lastUpdatedAt": "2020-11-13T14:22:08Z"
        }
    ],
    "nextCursor": "MTYwNTI3NzMyOHwxNjQ0MTNQMXxJTkFDVElWRQ=="
}
```

The example above gives the first five product records in the catalogue, with modification dates in chronological order. Note that products with a `"status"` of `"INACTIVE"` are products that have been temporarily disabled by the supplier and are therefore unavailable – when this is the case, all other requests for this product should be avoided. 

Included in the response is the `nextCursor` element, which contains an identification code that points to the next page of product records; i.e.:

```javascript
"nextCursor": "MTYwNTI3NzMyOHwxNjQ0MTNQMXxJTkFDVElWRQ=="
```

In your next call to [/products/modified-since](../../../openapi/reference/operation/productsModifiedSince), provide the value of `nextCursor` in the `cursor` parameter to get the next page of results:

```
GET https://api.sandbox.viator.com/partner/products/modified-since?count=500&cursor=MTYwNTI3NzMyOHwxNjQ0MTNQMXxJTkFDVElWRQ==
```

The response will be similar to the initial response shown above, except it will contain the **next** 500 product records and a new `nextCursor` that points to the following page, and so on.

Continue calling [/products/modified-since](../../../openapi/reference/operation/productsModifiedSince) using the `nextCursor` value in the `cursor` parameter to retrieve all pages of results. You will eventually receive a response that contains an empty `products` array and <u>does not</u> contain a `nextCursor` element. The absence of the `nextCursor` element indicates that you have, for the time being, reached the end of the list and have received all product records from our catalogue.

**Example final page response**

```javascript
{
    "products": []
}
```

## Periodically update your product records

Now that you have all product records from our catalogue, you can keep it up-to-date by periodically polling the service using the most-recent `nextCursor` code you received. Regardless of how frequently you call [/products/modified-since](../../../openapi/reference/operation/productsModifiedSince), you will always receive all product update information so long as you keep track of the last cursor you received and use that in your subsequent call.

You should never need to re-ingest the entire product catalogue unless you need to re-initialize your database. This may happen frequently during development, but never (or rarely) in production. Due to the large volume of data, we strongly recommend keeping it to a minimum.

Products are considered updated when the supplier makes any changes to the product's details, excluding pricing and availability changes, which are retrieved from the [availability schedule](../../key-concepts/content-ingestion-endpoints) endpoints. 

When the supplier publishes their product detail updates, the modified product [/products/modified-since](../../../openapi/reference/operation/productsModifiedSince) service will respond to this same call with newly-modified products in the `products` array and a new `nextCursor` element with which to poll the service for future updates in the same way.

It would be reasonable to poll this service on an hourly basis, updating those records in your local copy of the product catalogue as they become available.

Note that the `nextCursor` code is valid indefinitely; it will not expire.

## Rate limiting

We apply a rate limit of 150 requests per 10 second time window (rolling) for all requests to this API.

Request rates exceeding this limit will result in a HTTP 429 (Too Many Requests) status code being returned.

In order to avoid running-up against the rate limits, insert a delay of 2s if you receive a HTTP 429 status code. Furthermore, do not perform content ingestion as a multi-threaded process.

## Filtering out products

You are free to choose which products to store on your system and sell. As the product content endpoints return all available products, you will need to perform the filtering step yourself at the time of ingestion. If a product contains attributes that you do not desire; e.g., the type of product, where it operates, etc., simply discard the update and do not add it to your database.

### Filtering out manual-confirmation products

Unless you have established the required functionality on your site to sell [manual confirmation products](../../booking-concepts/booking-confirmation-types) you will need to **exclude** all non-instant confirmation products from your catalogue.

Instant confirmation products can be identified by the value of `bookingConfirmationSettings.confirmationType` being `"INSTANT"` in the response [product content response](../../key-concepts/content-ingestion-endpoints); e.g.:

```javascript
"bookingConfirmationSettings": {
  "bookingCutoffType": "CLOSING_TIME",
  "bookingCutoffInMinutes": 0,
  "confirmationType": "INSTANT"
}
```

Products wih a `confirmationType` of `"MANUAL"` or `"INSTANT_THEN_MANUAL"` should be excluded if you do not wish to sell manual confirmation products.