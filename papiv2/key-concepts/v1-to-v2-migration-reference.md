## V1 to V2 migration reference

### Product display page

This following diagram shows the different data element names in the responses from the v1 and v2 product content endpoints from which the sections of the [product display page for product 114491P5 on Viator.com](https://www.viator.com/tours/Niagara-Falls/All-American-Private-custom-Tour/d23183-114491P5) are sourced.

<div style="width: 100%; height: 60em; margin: 10px; position: relative;"><iframe allowfullscreen frameborder="0" style="width:100%; height:100%" src="https://app.lucidchart.com/documents/embeddedchart/d0426aab-8583-45d4-ad16-9b9153850fa0" id="02nKkFFz1Lqr"></iframe></div>

### Operational differences

#### v1's `treatAsAdult` flag

In v1 of this API, the requirement (or lack thereof) for an adult to be included in a booking was communicated via the `treatAsAdult` flag in the items of the `ageBands[]` array in the response from the [/product](https://docs.viator.com/partner-api/merchant/technical/#tag/Product-services/paths/~1product/get) endpoint. The default assumption in the v1 API was that at least one adult is required per booking unless the `treatAsAdult` flag is set to `true` for a non-adult age-band, in which case the inclusion of one traveler from that age-band is sufficient to fulfil the requirement.

See: [Working with age-bands](../../booking-concepts/working-with-age-bands/) for more information.

In v2 of this API, whether an adult traveler is required in order to make a booking is communicated via the `bookingRequirements.requiresAdultForBooking` flag in the response from any of the [product content endpoints](../content-ingestion-endpoints).

If this flag is `true`, at least one traveler from one of the `'ADULT'`, `'SENIOR'` or `'TRAVELER'` age-bands **must** be included when making the booking. 