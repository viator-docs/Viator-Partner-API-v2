# Access to endpoints

The API endpoints accessible to you depend on which kind of partner you are; affiliate or merchant. 

**Basic-access Affiliate partners** have access to a limited subset of the non-transactional endpoints of the Viator Partner API. The basic-access tier allows affiliates to quickly get started building their site, allows them to launch as soon as possible.

**Full-access Affiliate partners** have access to all non-transactional endpoints; i.e., everything except booking, booking hold and booking cancellation endpoints. In addition, full-access affiliates have access to additional legacy endpoints that provide functionality that helps to support attractions.

**Merchant partners** have access to all endpoints apart from the attraction endpoints designed for use by affiliate partners.

The following table describes which partner types have access to which endpoints:

| Endpoint | Basic-access Affiliate | Full-access Affiliate | Merchant |
|----------|:-----------------:|:----------------:|:------------:|
| [/products/{product-code}](#operation/products) | ❌ | ✅ | ✅ |
| [/products/modified-since](#operation/productsModifiedSince) | ❌ | ✅ | ✅ |
| [/products/bulk](#operation/productsBulk) | ❌ | ✅ | ✅ |
| [/products/tags](#operation/productsTags) | ✅ | ✅ | ✅ | 
| [/products/search](#operation/productsSearch) | ✅ | ✅ | ✅ |
| [/products/booking-questions](#operation/productsSearch) | ❌ | ❌ | ✅ |
| [/availability/check](#operation/checkAvailability) | ❌ | ✅ | ✅ |
| [/availability/schedules/{product-code}](#operation/availabilitySchedules) | ✅ | ✅ | ✅ |
| [/availability/schedules/modified-since](#operation/availabilitySchedulesModifiedSince) | ❌ | ✅ | ✅ |
| [/availability/schedules/bulk](#operation/availabilitySchedulesBulk) | ❌ | ✅ | ✅ |
| [/bookings/hold](#operation/bookingsHold) | ❌ | ❌ | ✅ |
| [/bookings/book](#operation/bookingsBook) | ❌ | ❌ | ✅ |
| [/bookings/status](#operation/bookingsBook) | ❌ | ❌ | ✅ |
| [/bookings/cancel-reasons](#operation/bookingsCancelReasons) | ❌ | ❌ | ✅ |
| [/bookings/{booking-reference}/cancel-quote](#operation/bookingsCancelQuote) | ❌ | ❌ | ✅ |
| [/bookings/{booking-reference}/cancel](#operation/bookingsCancel) | ❌ | ❌ | ✅ |
| [/locations/bulk](#operation/locationsBulk) | ✅ | ✅ | ✅ |
| [/exchange-rates](/#operation/exchangeRates) | ✅ | ✅ | ✅ |
| [/reviews/product](#operation/reviewsProduct) | ❌ | ✅ | ✅ |
| [/v1/taxonomy/destinations](#operation/v1TaxonomyDestinations) | ✅ | ✅ | ✅ |
| [/v1/taxonomy/attractions](#operation/v1TaxonomyAttractions) | ✅ | ✅ | ✅ |
| [/v1/product/photos](#operation/v1ProductPhotos) | ❌ | ✅ | ✅ |
| [/v1/search/attractions](#operation/v1SearchAttractions) | ✅ | ✅ | ❌ |
| [/v1/attraction](#operation/v1Attraction) | ❌ | ✅ | ❌ |
| [/v1/attraction/reviews](#operation/v1AttractionReviews) | ❌ | ✅ | ❌ |
| [/v1/attraction/photos](#operation/v1AttractionPhotos) | ❌ | ✅ | ❌ |
| [/v1/attraction/products](#operation/v1AttractionProducts) | ✅ | ✅ | ❌ |
| [/v1/support/customercare](#operation/v1SupportCustomercare) | ❌ | ✅ | ❌ |