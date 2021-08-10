# Cancellation policy

**Note**: This section applies to merchant partners only.

As well as *making* bookings, merchant partners are also able to *cancel* bookings through the Viator API using the [/bookings/cancel-reasons](../../../openapi/reference/operation/bookingsCancelReasons), [/bookings/{booking-ref}/cancel-quote](../../../openapi/reference/operation/bookingsCancelQuote) and [/bookings/{booking-ref}/cancel](../../../openapi/reference/operation/bookingsCancel) endpoints. Items cancelled via the [/bookings/{booking-ref}/cancel](../../../openapi/reference/operation/bookingsCancel) endpoint will be cancelled in full, and only one booking can be cancelled at a time.

For more information about how to perform cancellation using this API, see: [Cancellation API workflow](../cancellation-api-workflow).


## Cancellation policies

All products can be cancelled by the merchant partner; however, the refund granted by the supplier depends on the cancellation policy for the product in question.

There are <u>three</u> cancellation policy categories, **standard**, **custom** and **all sales final**, indicated by the `type` element of the `cancellationPolicy` object in the responses from the [product content endpoints](../../key-concepts/content-ingestion-endpoints).

**Note:** *These policies are those provided by Viator to you, the merchant partner. As the merchant of record, you can choose whether to extend these terms to your customers unchanged; or, set your own cancellation terms. For example, you might choose to make all products non-refundable; or, you might extend the full-refund cancellation window to 72 hours instead of 24 hours, and so forth. However, you will still be invoiced according to Viator's cancellation policies communicated via the API*

## Standard cancellation policy (`"STANDARD"`)
Products in this category can be cancelled up to 24 hours before the travel time (local supplier time) and a full refund will be granted. However, a <u>100% cancellation penalty</u> applies for cancellations submitted less than 24 hours before the start time. Most products (about 85%) fall into this category.

### Example response snippet

- **Source endpoint**: [/products/{product-code}](../../../openapi/reference/operation/products)
- **Product**: `5010SYDNEY`

```javascript
{
    "cancellationPolicy": {
        "type": "STANDARD",
        "description": "For a full refund, cancel at least 24 hours before the scheduled departure time.",
        "cancelIfBadWeather": false,
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
```

This product has the *standard* cancellation policy; i.e., when a booking is cancelled:

| Policy | `dayRangeMin` | `dayRangeMax` | Logic | `percentageRefundable` |
|--------|:-----------:|:-----------:|-------|:--------------------------:|
| *less than* <u>one</u> day (24 hours) before the start time | 0 | 1 | (product_start_time - cancellation_time) >= 0 days && (product_start_time - cancellation_time) < 1 days | 0 |
| *more than* <u>one</u> day (24 hours) before the start time | 1 | (absent) | (product_start_time - cancellation_time) >= 1 day | 100 |

## Custom cancellation policy (`"CUSTOM"`)
The refund amount for products in this category varies depending on how long before its start time the product is cancelled. Many products on a custom policy are multi-day tours, which require more sophisticated planning on the supplier’s end. Only a small number of products (around 5%) fall into this category.

 - **Note**: the `description` field contains a natural-language (and therefore language-localized) description of the policy described in the `refundEligibility` array. You can use this description for customer-messaging directly, or implement your own natural-language generation techique. With the cancellation policy encoded in a structured way, it would also be possible to display this information graphically.

### Example response snippet

- **Source endpoint**: [/products/{product-code}](../../../openapi/reference/operation/products)
- **Product**: `2264RJ410`

```javascript
"cancellationPolicy": {
        "type": "CUSTOM",
        "description": "If you cancel at least 30 day(s) in advance of the scheduled departure, there is no cancellation fee.<br>If you cancel between 10 and 29 day(s) in advance of the scheduled departure, there is a 50 percent cancellation fee.<br>If you cancel within 9 day(s) of the scheduled departure, there is a 100 percent cancellation fee.",
        "cancelIfBadWeather": false,
        "cancelIfInsufficientTravelers": false,
        "refundEligibility": [
          {
            "dayRangeMin": 10,
            "dayRangeMax": 30,
            "percentageRefundable": 50
          },
          {
            "dayRangeMin": 30,
            "percentageRefundable": 100
          },
          {
            "dayRangeMin": 0,
            "dayRangeMax": 10,
            "percentageRefundable": 0
          }   
        ]
    },
```

This product has a complex cancellation policy; where cancellations processed:

| Policy | `dayRangeMin` | `dayRangeMax` | Logic | `percentageRefundable` |
|--------|:-----------:|:-----------:|-------|:--------------------------:|
| <u>30</u> days or more before the start time | 30 | (absent) | (product_start_time - cancellation_time) >= 30 days | 100 |
| <u>10</u> days and *less than* <u>30</u> days (10 to 30 days) *before* the start time or more | 10 | 30 | (product_start_time - cancellation_time) >= 10 days && (product_start_time - cancellation_time) < 30 days | 50 |
| *less than* <u>10</u> days *before* the start time | 0 | 10 | (product_start_time - cancellation_time) < 10 days | 0 |

**Note:** When the `dateRangeMax` field is absent, this means infinity. Therefore, the second element in the `refundEligibility` array above indicates that the time period begins infinitely far in the past *until* 30 days prior to the tour or activity commencing.

## `3` – All sales final (100% cancellation penalty / no refund offered)

Products in this category cannot be cancelled or amended without incurring a 100% penalty; i.e., the refund amount will be zero. Around 10% of products fall into this category.

### Example response snippet

- **Source endpoint**: [/products/{product-code}](../../../openapi/reference/operation/products)
- **Product**: `5985P7`

```javascript
{
    "cancellationPolicy": {
        "type": "ALL_SALES_FINAL",
        "description": "All sales are final and incur 100% cancellation penalties",
        "cancelIfBadWeather": false,
        "cancelIfInsufficientTravelers": false,
        "refundEligibility": [
            {
                "dayRangeMin": 0,
                "percentageRefundable": 0
            }
        ]
    },
```

Products in this category can be cancelled, but no refund will be granted.

## startTimeStamp and endTimeStamp

Within the `cancellationPolicy` object in the response from [/bookings/book](../../../openapi/reference/operation/bookingsBook), the `refundEligibility.startTimestamp` and `refundEligibility.endTimestamp` fields contain time-stamps (UTC) that indicate the exact times between which the different cancellation refund rates apply.

**Note**: 
 * `refundEligibility.startTimestamp` will always be further in the past than `refundEligibility.endTimestamp`. 
 * Please use `startTimestamp` and `endTimestamp`, rather than `dayRangeMin` and `dayRangeMax`, to determine which cancellation policy is in effect.

**Example response snippet from [/bookings/book](../../../openapi/reference/operation/bookingsBook)**:

```javascript
"cancellationPolicy": {
  "type": "STANDARD",
  "description": "For a full refund, cancel at least 24 hours before the scheduled departure time.",
  "cancelIfBadWeather": false,
  "cancelIfInsufficientTravelers": false,
  "refundEligibility": [
    {
      "dayRangeMin": 1,
      "percentageRefundable": 100,
      "startTimestamp": "2020-08-25T00:36:49.69005Z",
      "endTimestamp": "2020-11-28T13:00:00Z"
    },
    {
      "dayRangeMin": 0,
      "dayRangeMax": 1,
      "percentageRefundable": 0,
      "startTimestamp": "2020-11-28T13:00:00Z",
      "endTimestamp": "2020-11-29T13:00:00Z"
    }
  ]
}
```

## Post-travel cancellations

Occasionally, customers seek a refund for a product **after** completing their travels.

The reason for this might be because they were unable to attend the tour due to the supplier having cancelled the tour due to bad weather or other reasons beyond the customer's control; or, the customer might have been extremely dissatisfied with the tour itself, felt that it was misrepresented in its advertising, or some other serious complaint.

When this occurs, you will need to [send a refund request by email to dpsupport](mailto:dpsupport@viator.com) and include both "CANCEL" and the booking reference number in the subject line.

For **all** post-travel cancellation requests, you will need to include a detailed description of the issue. 

Except in cases of known service interruptions (e.g., due to extreme weather events), we will first verify the issue and seek authorization from the product supplier. 

Once a decision regarding the refund has been made, we will notify your Customer Services Department with this information. You will then need to advise your customer directly and process the refund if granted.

## Partial refunds

While we recommend that merchant partners support the processing of partial refunds for their customers, it is ultimately up to the partner whether to implement this functionality. 

If you would prefer to only grant the full (100%) refund that is offered on most products so long as the cancellation is processed more than 24 hours prior to the product's start time, we recommend that you implement logic that checks whether a 100% refund is available for the product at the time the customer wishes to cancel their booking.

| **Type 1: Standard policy** (`cancellationPolicy.type` is `"STANDARD"`) |
|-------------------------------------------------------------|
| The 100% refund is available so long as the cancellation is performed more than 24 hours prior to the product start time |

| **Type 2: Custom policy** (`cancellationPolicy.type` is `"CUSTOM"`) |
|-----------------------------------------------------------|
| You will need to check whether any of the canellation policy objects in the `refundEligibility` array have: <ul><li>a `percentageRefundable` value that is non-zero, and</li><li>`dayRangeMin` and `dayRangeMax` describe an epoch that includes the present time</li></ul> |

| **Type 3: All sales final** (`cancellationPolicy.type` is `"ALL_SALES_FINAL"`)|
|-------------------------------------------------------------|
| No refunds are available (except in the case of manual confirmation products that are still in a 'pending' state and special exceptions for post-trip cancellations); therefore, under normal conditions, if you grant a refund to your customer for this kind of product, it will be solely at *your* expense (i.e., you will still be invoiced for the cost of the product by Viator). Therefore, we recommend that you do not allow refunds for products with this policy. |