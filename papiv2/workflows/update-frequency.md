# Update frequency

## Fixed-cadence delta updates

In order to keep your local databases up-to-date without placing an excessive burden on our servers, we recommend the following fixed cadences at which you should poll the content-ingestion endpoints:

| Endpoint | Recommended update cadence |
|----------|---------------------|
| [/products/modified-since](../../../openapi/reference/operation/productsModifiedSince) | Every 15-30 minutes following initial ingestion |
| [/availability/schedules/modified-since](../../../openapi/reference/operation/productsModifiedSince) | Every 15-30 minutes following initial ingestion |

## Fixed-cadence full updates

To ensure your systems reflect any removals of or changes to existing destinations, locations or booking questions, we recommend retrieving full updates from these endpoints as follows:


| Endpoint | Recommended update cadence |
|----------|---------------------|
| [/v1/taxonomy/destinations](../../../openapi/reference/operation/v1TaxonomyDestinations) | Daily |
| [/v1/taxonomy/attractions](../../../openapi/reference/operation/v1TaxonomyAttractions) | Daily |
| [/products/booking-questions](../../../openapi/reference/operation/productsBookingQuestions) | Daily |
| [/products/tags](../../../openapi/reference/operation/productsTags) | Daily | 
| [/locations/bulk](../../../openapi/reference/operation/locationsBulk) | Weekly |
| [/reviews/product](#operation/reviewsProduct) | Weekly |

## On-demand updates

When ingesting product content, in the event that you encounter an unknown reference – i.e., a new location reference, booking question, tag or destination id – or, if you need to perform a currency conversion for which the last exchange rate you retrieved has expired, we recommend you call the relevant endpoint to resolve the new reference immediately or just after completing the product content update.

| Endpoint | When to update |
|----------|-------------|
| [/exchange-rates](../../../openapi/reference/operation/exchangeRates) | Whenever you encounter a currency that you need to convert, but the last-retrieved exchange-rate for that currency-pair is no longer valid due to having expired (according to the `expiry` value in the response from this endpoint) |
| [/locations/bulk](../../../openapi/reference/operation/locationsBulk) | Whenever you encounter a location reference code that you do not yet have the details for (we recommend retrieving location details in batches using this endpoint; therefore, the retrieval of new location data can commence after all new product content is retrieved) |
| [/products/tags](../../../openapi/reference/operation/productsTags) | Whenever you encounter a tag reference code that you do not yet have the details for |
| [/v1/taxonomy/destinations](../../../openapi/reference/operation/v1TaxonomyDestinations) | Whenever you encounter a destination id that you do not yet have the details for |
| [/products/booking-questions](../../../openapi/reference/operation/productsBookingQuestions) | Whenever you encounter a booking question identifier that you do not yet have the details for |
| [/v1/taxonomy/attractions](../../../openapi/reference/operation/v1TaxonomyAttractions) | Whenever you encounter an attraction identifier that you do not yet have the details for |
| [/reviews/product](#operation/reviewsProduct) | If the number of reviews available for a product, which is reported in the `reviews.totalReviews` element in the [product content response](#section/Key-concepts/Content-ingestion-endpoints), has changed compared with its previous value, the reviews for that product should also be refreshed by calling the [/reviews/product](#operation/reviewsProduct) endpoint. We request that you rate-limit your use of this service to 30 requests per minute. |