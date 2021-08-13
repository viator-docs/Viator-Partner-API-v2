# Access to endpoints

The API endpoints accessible to you depend on which kind of partner you are; affiliate or merchant. 

**Affiliate partners** have access to all non-transactional endpoints of the Viator Partner API. I.e., everything except booking, booking hold and booking cancellation endpoints. In addition, affiliates have access to additional legacy endpoints that provide functionality that helps to support attractions.

**Merchant partners** have access to all endpoints apart from the attraction endpoints designed for use by affiliate partners.

The following table describes which partner types have access to which endpoints:

| Endpoint | Affiliate partners | Merchant partners |
|----------|:-----------------:|:----------------:|
| [/products/{product-code}](../../openapi/reference/operation/products) | ✅ | ✅ |
| [/products/modified-since](../../openapi/reference/operation/productsModifiedSince) | ✅ | ✅ |
| [/products/bulk](../../openapi/reference/operation/productsBulk) | ✅ | ✅ |
| [/products/tags](../../openapi/reference/operation/productsTags) | ✅ | ✅ | 
| [/products/search](../../openapi/reference/operation/productsSearch) | ✅ | ✅ |
| [/products/booking-questions](../../openapi/reference/operation/productsSearch) | ❌ | ✅ |
| [/availability/check](../../openapi/reference/operation/checkAvailability) | ✅ | ✅ |
| [/availability/schedules/{product-code}](../../openapi/reference/operation/availabilitySchedules) | ✅ | ✅ |
| [/availability/schedules/modified-since](../../openapi/reference/operation/availabilitySchedulesModifiedSince) | ✅ | ✅ |
| [/availability/schedules/bulk](../../openapi/reference/operation/availabilitySchedulesBulk) | ✅ | ✅ |
| [/bookings/hold](../../openapi/reference/operation/bookingsHold) | ❌ | ✅ |
| [/bookings/book](../../openapi/reference/operation/bookingsBook) | ❌ | ✅ |
| [/bookings/status](../../openapi/reference/operation/bookingsBook) | ❌ | ✅ |
| [/bookings/cancel-reasons](../../openapi/reference/operation/bookingsCancelReasons) | ❌ | ✅ |
| [/bookings/{booking-reference}/cancel-quote](../../openapi/reference/operation/bookingsCancelQuote) | ❌ | ✅ |
| [/bookings/{booking-reference}/cancel](../../openapi/reference/operation/bookingsCancel) | ❌ | ✅ |
| [/locations/bulk](../../openapi/reference/operation/locationsBulk/) | ✅ | ✅ |
| [/exchange-rates](../../openapi/reference/operation/exchangeRates) | ✅ | ✅ |
| [/reviews/product](../../openapi/reference/operation/reviewsProduct) | ✅ | ✅ |
| [/v1/taxonomy/destinations](../../openapi/reference/operation/v1TaxonomyDestinations) | ✅ | ✅ |
| [/v1/taxonomy/attractions](../../openapi/reference/operation/v1TaxonomyAttractions) | ✅ | ✅ |
| [/v1/product/photos](../../openapi/reference/operation/v1ProductPhotos) | ✅ | ✅ |
| [/v1/search/attractions](../../openapi/reference/operation/v1SearchAttractions) | ✅ | ❌ |
| [/v1/attraction](../../openapi/reference/operation/v1Attraction) | ✅ | ❌ |
| [/v1/attraction/reviews](../../openapi/reference/operation/v1AttractionReviews) | ✅ | ❌ |
| [/v1/attraction/photos](../../openapi/reference/operation/v1AttractionPhotos) | ✅ | ❌ |
| [/v1/attraction/products](../../openapi/reference/operation/v1AttractionProducts) | ✅ | ❌ |
| [/v1/support/customercare](../../openapi/reference/operation/v1SupportCustomercare) | ✅ | ❌ |