# Working with age bands

## Why have age bands?

Tour and experience product suppliers can set different prices for (and impose different rules on) customers according to how old they are. 

For example, suppliers might choose to charge people 18 years and older ('adults') the full ticket price, while 'children' can book at a lower price. 

Or, the tour operator may only allow children to make a group booking for the tour so long as the group contains 'at least one adult'.

Viator provides six age-band categories that product suppliers can use to segregate travelers into age groups (the limits of which they also define) in order to set pricing and traveler-count participation rules for their product according to the age band categories.

The age-bands available for a particular product, such as *adult*, *child*, *infant*, etc., are returned by the [/products/{product-code}](../../../openapi/reference/operation/products) service. Your customer should be able to select a different number of people from each available age-band during the price check and checkout process.

## Age-band categories

The age bands supported by the Viator API are as follows:

| bandId | Description |
|------:|-------|
| `"ADULT"` | Adult |
| `"CHILD"` | Child |
| `"INFANT"` | Infant |
| `"YOUTH"` | Youth |
| `"SENIOR"` | Senior |
| `"TRAVELER`" | Catch-all age-band for unit-priced products |

Note that the `"TRAVELER"` is only used for bookings with [unit pricing](../per-person-and-unit-pricing), in which case it will be the only age-band available.

The exact age range to which each category pertains is defined by the supplier, and the maximum and minimum ages that each age band describes for each product can be found in the product content response; e.g.:

```javascript
"ageBands": [
  {
      "ageBand": "INFANT",
      "startAge": 0,
      "endAge": 2,
      "minTravelersPerBooking": 0,
      "maxTravelersPerBooking": 15
  },
  {
      "ageBand": "CHILD",
      "startAge": 3,
      "endAge": 11,
      "minTravelersPerBooking": 0,
      "maxTravelersPerBooking": 15
  },
  {
      "ageBand": "ADULT",
      "startAge": 12,
      "endAge": 80,
      "minTravelersPerBooking": 1,
      "maxTravelersPerBooking": 15
  }
]

```

For this product, the age bands have been defined as follows:

| `ageBand` | `ageFrom` | `ageTo` |
|:--------:|:-------:|:-----:|
| ADULT | 13 | 64 |
| SENIOR | 65 | 99 |
| CHILD | 4 | 12 |


Product suppliers must define at least one age band for their tour, and there are no 'default' age ranges. Therefore, if the supplier has only specified a single 'adult' age band covering ages 18-99, it must be assumed that only people aged 18-99 are eligible to book the tour, essentially excluding children and centenarians in this case.

## When you will use age-bands

The age-band of a customer needs to be communicated via the API in the following situations:

1. When placing a booking-hold
  - you will need to supply the age-band of each of the travelers being booked for in the `paxMix` element in the request to [/bookings/hold](../../../openapi/reference/operation/bookingsHold).
2. When making a booking 
  - As with the booking hold, age-bands must be supplied in the `paxMix` element 
  - If the supplier has specified `"AGEBAND"` as a booking question, you must additionally provide these details in the `bookingQuestionAnswers` array in the request to [/bookings/book](../../../openapi/reference/operation/bookingBook). Note that these details are verified by the booking server and the booking will be rejected if the details do not match. For more information about answering the `"AGEBAND"` booking question, see [Booking concepts – Booking questions](../booking-questions).