# Product options

Any particular product may consist of a number of variants, each of which is referred to as a 'product option'. In previous versions of this API, and in the tours and activities sector in general, product options are also referred to as 'tour grades'.

For example, product options might represent different departure times, tour routes, or packaged extras like additional meals, transport and so forth.

The product options available for a product can be found in the `productOptions` array in the responses from any of the [product content endpoints](../content-ingestion-endpoints).
 
An example `productOptions` array for the product **5010SYDNEY** is as follows:

```javascript
"productOptions": [
    {
        "productOptionCode": "48HOUR",
        "description": "Duration: 2 days: FREE BONUS ENTRY to Sydney Tower with every Deluxe ticket end 31st March<br/>48 Hour Premium Ticket: Unlimited use on Big Bus Sydney & Bondi Tour for 48 hours from time of first use PLUS a guided walking tour of The Rocks, Syd",
        "title": "48 Hour Premium Ticket ",
        "languageGuides": [...]
    },
    {
    "productOptionCode": "TG1",
        "description": "Hop-on Hop-Off and Attractions: 48hr Big Bus Tours, 1-day Hop-On Hop-Off Harbour Cruise, 4 Attractions: Tower Eye, Madame Tussauds, Wildlife Zoo, Aquarium<br/>Duration: 2 days<br/>Complimentary Walking Tours: Complimentary English-speaking guided walking tour of “The Rocks” historic and harbourside precinct.",
        "title": "48Hour Deluxe PLUS Attractions",
        "languageGuides": [...]
    },
    {
        "productOptionCode": "24HOUR",
        "description": "Unlimited use on Big Bus Sydney & Bondi Hop-on Hop-off Tour for 24 hours from time of first use",
        "title": "24 Hour Classic Ticket ",
        "languageGuides": [...]
    },
    {
        "productOptionCode": "DELUXE",
        "description": "Big Bus and Habour Cruise: Combine two great Sydney experiences into one with a hop-on hop off Big Bus Tours and a hop-on hop-off Sydney Harbour cruise <br/>Duration: 2 days: FREE BONUS ENTRY to Sydney Tower with every Deluxe ticket end 31st March<br/>Complimentary Walking Tour: Complimentary English-speaking 90-minute guided walking tour of “The Rocks” historic and harbourside precinct.",
        "title": "48 Hour Deluxe Bus and Cruise",
        "languageGuides": [...]
    }
]
```

This product has four product options, each with an identifying code given in the `productOptionCode` field:

- `"48HOUR"`
- `"TG1"`
- `"24HOUR"`
- `"DELUXE"`

Not all products have multiple product options. Products with no variations will not include a `productOptions` element in the response.

Product options are essentially a subcategory of the tour or activity. You will need to specify the product option you wish to book when making a booking.