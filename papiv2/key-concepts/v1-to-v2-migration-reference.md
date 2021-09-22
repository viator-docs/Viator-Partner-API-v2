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

### Mapping categories to tags

In version 2 of the Viator API, the concept of 'product categories' that was utilized in version 1 has been replaced by tags.

In version 1, the list of categories and subcategories was available on a per-destination basis using v1's [/taxonomy/cateogories](https://docs.viator.com/partner-api/merchant/technical/#operation/taxonomyCategories) endpoint. This list could be used to construct a category tree for display to the user; or, to aid in search.

In version 2, tags can be used to replicate this functionality simply by mapping the original v1 subcategory to its v2 tag equivalent. Tag information is available from the [/products/tags](#operation/productsTags) endpoint.

These mappings are detailed in this Excel spreadsheet: [v1 category and subcategory mappings to v2 tags](https://docs.viator.com/partner-api/technical/v1-subcategories-v2-tags.xlsx). 

All v2 tags that correspond to categories or subcategories in v1 are listed in the spreadsheet, which can be interpreted as per this example:

| v2 tag display name | v2 tagId | v1 subcategoryId | v1 subcategoryName | v1 categoryId | v1 groupName |
|---------------------|----------|------------------|--------------------|--------------|---------------|
| Hot Air Balloon Rides | 12027 | 3 | Balloon Rides | 1 | Air, Helicopter & Balloon Tours |

The example above shows that the v2 tag "Hot Air Balloon Rides", which has a `tagId` of `12027`, corresponds to v1's "Balloon Rides" (subcategory 3), which falls under "Air, Helicopter & Balloon Tours" (category 1).

In the response from the v2 endpoint [/products/tags](../../../openapi/reference/operation/productsTags), "Hot Air Balloon Rides" (`tagId`: 12027) is shown here:

```javascript
{
    "tagId": 12027,
    "parentTagIds": [
        21715,
        21913,
        21436,
        21909
    ],
    "allNamesByLocale": {
        "de": "Heißluftballonfahrten",
        "no": "Turer i varmluftballong",
        "sv": "Flygturer i varmluftsballong",
        "pt": "Passeios de balão",
        "ko": "열기구 타기",
        "en_AU": "Hot Air Balloon Rides",
        "en": "Hot Air Balloon Rides",
        "it": "Giri in mongolfiera",
        "fr": "Vols en montgolfière",
        "en_UK": "Hot Air Balloon Rides",
        "es": "Paseos en globo aerostático",
        "zh": "热气球之旅",
        "zh_HK": "熱氣球之旅",
        "zh_TW": "熱氣球之旅",
        "ja": "熱気球での飛行体験",
        "zh_CN": "热气球之旅",
        "da": "Varmluftballonture",
        "nl": "Ballonvaarten"
    }
} 
```

This corresponds to the following result (Balloon-rides; category 1, subcategory 3) in the response from the v1 endpoint [/taxonomy/categories](https://docs.viator.com/partner-api/merchant/technical/#tag/Taxonomy-services):

```javascript
{
    "sortOrder": 1,
    "thumbnailURL": "https://hare-media-cdn.tripadvisor.com/media/attractions-splice-spp-154x109/06/75/b7/96.jpg",
    "subcategories": [
        {
            "sortOrder": 1,
            "subcategoryUrlName": "Balloon-Rides",
            "subcategoryId": 3,
            "subcategoryName": "Balloon Rides",
            "categoryId": 1
        },
        {
            "sortOrder": 2,
            "subcategoryUrlName": "Air-Tours",
            "subcategoryId": 1,
            "subcategoryName": "Air Tours",
            "categoryId": 1
        },
        {
            "sortOrder": 3,
            "subcategoryUrlName": "Helicopter-Tours",
            "subcategoryId": 2,
            "subcategoryName": "Helicopter Tours",
            "categoryId": 1
        }
    ],
    "thumbnailHiResURL": "https://hare-media-cdn.tripadvisor.com/media/attractions-splice-spp-674x446/06/75/b7/96.jpg",
    "productCount": 76,
    "groupName": "Air, Helicopter & Balloon Tours",
    "groupUrlName": "Air-Helicopter-and-Balloon-Tours",
    "id": 1
},

```

Using the spreadsheet, as above, you can replicate the category tree and categorise products accordingly using a product's tags, which are provided in the product content response.