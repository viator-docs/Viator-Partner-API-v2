# Update log

## Previous changes

| Date | Description |
|------|-------------|
| 23 Mar 2021 | Added [Resolving references](../../workflows/resolving-references) section |
| 11 Mar 2021 | Added `campaign-value` parameter to [/products/modified-since](../../../openapi/reference/operation/productsModifiedSince), [/products/bulk](../../../openapi/reference/operation/productsBulk) and [/products/{product-code}](../../../openapi/reference/operation/products) endpoints; added AvailabilityScheduleSummary `summary` schema to /availability/schedules/* endpoint responses |
| 16 Feb 2021 | Added [Determining ratings](../../workflows/determining-ratings) section; added [Conventions - Accept-Encoding](../../conventions/accept-encoding) section |
| 21 Jan 2021 | Extended BookingBookRequest schema (used in [/bookings/book](../../../openapi/reference/operation/bookingsBook)) to include new AdditionalBookingDetails schema to allow custom voucher text |
| 18 Jan 2021 | Modified `rejectionReasonCode` in responses from [/bookings/book](../../../openapi/reference/operation/bookingsBook) and [/bookings/status](../../../openapi/reference/operation/bookingsStatus) endpoints |
| 12 Jan 2021 | Added [Key concepts – Booking confirmation types](../../booking-concepts/booking-confirmation-types) section as sale of manual confirmation products has now been enabled. |
| 7 Dec 2020 | Added DayOperatingHours schema array to BookableItemSeason schema in availability-schedules endpoints | 
| 24 Nov 2020 | Added [Key concepts – Booking cutoff times](../../booking-concepts/booking-cutoff-times) section |
| 12 Nov 2020 | Added [Workflows – Checking booking status](../../booking-concepts/checking-booking-status) section |
| 6 Nov 2020 | Added [/bookings/status](../../../openapi/reference/operation/bookingsStatus) endpoint specification |
| 5 Nov 2020 | Added [Workflows – Ingesting and updating availability schedules](../../workflows/ingesting-and-updating-availability-schedules) section |
| 4 Nov 2020 | Updated `id` values in [/products/booking-questions](../../../openapi/reference/operation/productsBookingQuestions) endpoint |
| 3 Nov 2020 | Added [Key concepts – V1 to V2 migration reference](../../key-concepts/v1-to-v2-migration-reference) section |
| 28 Oct 2020 | Added sections for [Activity](../../key-concepts/itineraries/#activity-itinerary), [Hop-on hop-off](../../key-concepts/itineraries/#hop-on--hop-off-itinerary) and [Unstructured](../../key-concepts/itineraries/#unstructured-itinerary) itinerary types |
| 13 Oct 2020 | - Added `maxLength` field to BookingQuestion schema in response from [/products/booking-questions](../../../openapi/reference/operation/productsBookingQuestions)<br /> - Updated AdditionalInfo schema |
| 6 Oct 2020 | - Added [Appendices - Inclusions and exclusions](../../appendices/inclusions-and-exclusions)<br /> - Modified ActiveProduct schema to include new required `Supplier` element |
| 1 Oct 2020 | Added [About](#section/About) section |
| 29 Sep 2020 | Added [Key concepts - Availability Schedules](../../key-concepts/availability-schedules) section<br />Added legacy helper endpoints [/v1/taxonomy/destinations](../../../openapi/reference/operation/v1TaxonomyDestinations), [/v1/taxonomy/attractions](../../../openapi/reference/operation/v1TaxonomyAttractions), [v1/product/reviews](../../../openapi/reference/operation/v1ProductReviews), [/v1/product/photos](../../../openapi/reference/operation/v1ProductPhotos) |
| 28 Sep 2020 | Updated response schema for [/products/tags](../../../openapi/reference/operation/productsTags) |
| 23 Sep 2020 | Added [Key concepts – Booking questions](../../booking-concepts/booking-questions) section |
| 22 Sep 2020 | Added [Versioning](../../conventions/api-versioning-strategy) section |
| 22 Sep 2020 | Fixed typo in StandardItineraryItem schema (`pointsOfInterestLocation` -> `pointOfIterestLocation`); added explanation of policy window time-stamps & updated response samples in [Workflows - Cancellation policy](../../booking-concepts/cancellation-policy) section | 
| 21 Sep 2020 | Added [Making a booking](../../booking-concepts/making-a-booking) section under Workflows |
| 20 Sep 2020 | Added `currency` element to [/bookings/hold](../../../openapi/reference/operation/bookingsHold) response schema |
| 17 Sep 2020 | Breaking change: [/exchange-rates](/../../../openapi/reference/operation/exchangeRates) endpoint refactored |
| 16 Sep 2020 | <mark>**Note**: ONLY freesale products (those with a confirmation type of "INSTANT") are presently able to be booked. See [Ingesting and updating the product catalogue](../../workflows/ingesting-and-updating-the-product-catalogue) (Filtering-out on-request products) for instructions on how to filter-out on-request products from your catalogue</mark> |
| 16 Sep 2020 | Breaking change: `languageGuideDetails` element in ActiveProduct schema changed to `languageGuides` (array of LanguageGuide schema) |
| 15 Sep 2020 | Breaking change: removed `travelerPickup` element from [/booking/book](../../../openapi/reference/operation/bookingsBook) request |
| 10 Sep 2020 | Added [/bookings/book](../../../openapi/reference/operation/bookingsBook) endpoint specification |
| 1 Sep 2020 | Added `description` element to `MultiDayTourFoodAndDrinks` and `InclusionExclusionItem` schemata; added `version=2.0` to `Accept` request header parameter across all endpoints |
| 26 Aug 2020 | `CancellationConditionType` schema key strings updated (now "STANDARD", "MODERATE", "STRICT" & "ALL_SALES_FINAL"; `PricingLineItem` updated (affects /availability/check and /bookings/hold-confirm responses); /availability/* endpoints moved to separate 'Availability' section |
| 25 Aug 2020 | <mark>**Breaking changes**</mark>: see below |
| 25 Aug 2020 | Regenerated response samples |
| 11 Aug 2020 | Updated `paxMix` schema in [/availability/check](../../../openapi/reference/operation/checkAvailability) |
| 6 Aug 2020 | Added COVID-safety-measure keys to `additionalInfo` schema |
| 4 Aug 2020 | Added [/bookings/hold](../../../openapi/reference/operation/bookingsHold) endpoint |
| 28 July 2020 | Added [Ingesting and updating the product catalogue](../../workflows/ingesting-and-updating-the-product-catalogue) section |
| 23 July 2020 | Added [Product options](), [Cancellation policy](../../booking-concepts/cancellation-policy) sections | 
| 16 Jul 2020 | Added [/bookings/hold](../../../openapi/reference/operation/bookingsHold) endpoint |
| 3 Jul 2020 | Modified availability endpoint schema and regenerated examples |
| 1 Jul 2020 | Removed internal-only markup for release |
| 16 Jun 2020 | Added [Workflows->Cancellation API workflow](../../booking-concepts/cancellation-api-workflow) section
| 15 Jun 2020 | Removed enum data type specification from `UnavailableDate`, `ageBand`, `CancellationQuoteBookingStatus`, `CancellationResultItemReason` and `CancellationBookingStatus` schemata |
| 18 May 2020 | Refactored LHS nav to use 'summary' name instead of operationId name |
| 28 April 2020 | Added `legacyGuide` field to `LanguageGuide` object specification |

## 25 August breaking changes

The following breaking changes have been made. Please update your OpenAPI specification by downloading from the link at the top of the page.

### Endpoint URL changes

- /availability-schedule/{product-code} -> [/availability/schedules/{product-code}](../../../openapi/reference/operation/availabilitySchedules)
- /availability-schedule/bulk -> [/availability/schedules/bulk](/../../../openapi/reference/operation/availabilitySchedulesBulk)
- /availability-schedule/modified-since -> [/availability/schedules/modified-since](../../../openapi/reference/operation/availabilitySchedulesModifiedSince)

### Element name changes

A number of name changes have been made to the `Product` schema. This affects the following endpoints:

- [/products/modified-since](../../../openapi/reference/operation/productsModifiedSince)
- [/products/bulk](../../../openapi/reference/operation/productsBulk)
- [/products/{product-code}](../../../openapi/reference/operation/products)

| Element | Old name | New name |
|----------|---------|----------|
| `cancellationPolicy.badWeather` |  `badWeather` | `cancelIfBadWeather` |   
| `cancellationPolicy.insufficientTravelers` | `insufficientTravelers` | `cancelIfInsufficientTravelers` |
| `translationInfo.translationAttributions` | `translationAttributions` | `translationAttribution` | 
| `destinations.isPrimary` | `isPrimary` | `primary` |
| `inclusions[].otherText` | `otherText` | `otherDescription` |   
| `itinerary.routes[].pois` | `pois` | `pointsOfInterest` |
| `itinerary.itineraryItems[].poiLocation` | `poiLocation` | `pointsOfInterestLocation` |
| `itinerary.routes[].duration.fixedValueinMinutes` | `fixedValueInMinutes` | `fixedDurationInMinutes` |
| `itinerary.routes[].duration.fromMinutes` | `fromMinutes` | `variableDurationFromMinutes` |
| `itinerary.routes[].duration.toMinutes` | `toMinutes` | `variableDurationToMinutes` |