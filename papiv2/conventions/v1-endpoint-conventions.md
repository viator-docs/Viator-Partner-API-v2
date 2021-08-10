# V1 endpoint conventions

There are ten endpoints necessary for the seamless operation of this API that have not yet been updated to the 'v2-style' functionality of the other endpoints. 

V1 endpoints available to both partner types:

- [/v1/taxonomy/destinations](../../../openapi/reference/operation/v1TaxonomyDestinations) - gets all destination names and their associated details, including `destinationId`
- [/v1/taxonomy/attractions](../../../openapi/reference/operation/v1TaxonomyAttractions) - gets all attractions for a destination, including `attractionId`
- [/v1/product/photos](../../../openapi/reference/operation/v1ProductPhotos) - gets all customer-submitted photos for a product


V1 endpoints for affiliate partners only:

- [/v1/search/attractions](../../../openapi/reference/operation/v1SearchAttractions) - retrieves a list of attractions associated with the given destination
- [/v1/attraction](../../../openapi/reference/operation/v1Attraction) – returns the details of an attraction
- [/v1/attraction/reviews](../../../openapi/reference/operation/v1AttractionReviews) - gets reviews related to an attraction 
- [/v1/attraction/photos](../../../openapi/reference/operation/v1AttractionPhotos) – returns photos that are related to an attraction
- [/v1/attraction/products](../../../openapi/reference/operation/v1AttractionProducts) – gets products that are related to an attraction
- [/v1/support/customercare](../../../openapi/reference/operation/v1SupportCustomercare) – returns the URL to the Viator help page


These ten v1 endpoints operate differently to the v2 endpoints. 

In particular:

- Standard error information is included at the top level of the response object
- Response payload is contained in the `data` element
- Success or failure is represented by the `success` element being `true` or `false`, **not** the HTTP status code, which will be `200` in every case in which a response was able to be returned, regardless of any error(s)
- Naming and other conventions may be different to what is expected – please review the specification carefully
- It is not necessary to include version information in the request header when calling these endpoints (see: [API versioning strategy](../api-versioning-strategy))

**Example V1 error response**:

```javascript
{
    "errorReference": "C0AA8A15:ED5E_0A2806D6:01BB_5F755F7F_54AA9:03D6",
    "data": null,
    "dateStamp": "2020-09-30T21:47:59+0000",
    "errorType": "EXCEPTION",
    "errorCodes": [
        "UNKNOWN_ERROR"
    ],
    "errorMessage": [
        null
    ],
    "errorName": "NullPointerException",
    "extraInfo": {},
    "extraObject": null,
    "success": false,
    "totalCount": 1,
    "errorMessageText": [],
    "vmid": "331009"
}
```