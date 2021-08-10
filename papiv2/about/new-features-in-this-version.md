# New features in this version

The Viator Partner-API v2.0 is a fully-redesigned system with regard to our previous API products. It includes all key fuctionality available in previous versions, but now provides nearly fully-structured data elements, and a more modern, concise and easily-understandable interface.

In addition, we have made significant improvements to performance across all functions of the API, and particularly in the area of product content and availability data ingestion.

## Simplified and optimized data ingestion 

Data ingestion has been greatly improved over previous versions of this API. Now, partners need only perform a single initial ingestion of data; then, only new and updated product content, availability and pricing information is retrieved as a 'delta update' as it becomes available.

In addition:
- A single end-point allows both bulk ingestion and delta updates, which can be controlled using a pagination cursor or with a time-stamp parameter to allow for point-in-time ingestion of any new updates and easy recovery from ingestion failure
- The structure of a product's pricing and availability schedules has been simplified to reduce response size

### Product content

- All pricing is standardized to the supplier's currency, avoiding the need to update on account of exchange rate fluctuations 
- Structured location, tag, booking question, review and photo data is available from separate endpoints to allow for parallel ingestion, and resusable data can be ingested once and applied globally

### Availability schedule information

Improvements to availability schedule ingestion performance and usability have been achieved:

- Availability is now communicated by providing an overall schedule season and specifying *unavailable* dates instead of available dates, a significantly more compact format that improves transfer speeds and minimizes storage needs  

- Availability and pricing information is given on a days-of-the-week basis, which can help with filtering and improving visibility of product availability for customers.

- Information about special pricing periods that may be running throughout a product's seasons is included and is easy to interpret, allowing partners to surface promotions to customers.

- Accurate pricing and applicability information for products that have 'per unit' (group) pricing is now included in the availability & pricing response; whereas support for this pricing model was limited in previous versions of this API. This allows partners to improve pricing exposure for these products.

## Structure-rich content

Providing structured content is a major focus of this API. Product information that was previously stored as natural-language descriptions in a single, lengthy field is now represented in intuitively-structured, machine-interpretable schemata that empowers partners to apply finely-grained merchandising control, including:

- Tour itineraries with comprehensive location data to allow for graphical display and advanced search
- Tour details, inclusions and dynamic health & safety information to support changing requirements due to global pandemic response initiatives
- Machine-parseable booking questions and answers

## Improved real-time availability and pricing checking

The [real-time availability check endpoint](../../../openapi/reference/operation/availabilityCheck):
- Allows you to check availability and pricing in real-time for a specific product / [product-option](#section/Key-concepts/Product-options) / date / starting time / pax mix combination
- Returns the ticket availability status along with a pricing breakdown (by age band) and total price (all age bands) in a simplified format to reduce response size
- Pricing components are stated explicitly, including the Recommended Retail Price (RRP), partner net price, special-offer pricing and booking fee component details

## Improved booking workflow

The [booking workflow](/papiv2/booking-concepts/making-a-booking/) now includes an optional booking-hold step, which allows you greater predictability of a successful booking, thereby increasing conversion rates by decreasing friction in the booking flow.

The [booking hold](../../../openapi/reference/operation/bookingsHold) endpoint supports two kinds of hold:
- Inventory/availability hold (increased likelihood of receiving a booking confirmation)
- Pricing hold (mitigates against pricing changes at the time of booking)

The [real-time availability and pricing endpoint](/openapi/reference/operation/availabilityCheck) also delivers \~5% faster performance than previous versions of this API.