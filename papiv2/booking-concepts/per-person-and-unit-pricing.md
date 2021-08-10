# Per-person and unit pricing

The products in our catalogue fall into two pricing categories: **per-person** and **unit** pricing.

A product's pricing category is given in the `pricingInfo.type` field of the product content response. Depending on whether the product has per-person or unit pricing, this value will be `"PER_PERSON"` or `"UNIT"`, respectively.

## Per-person pricing

If the pricing is *per-person*, then the total price of the booking will be directly proportional to the number of participants (passengers) of each age-band type that are being booked for the product; i.e., a direct multiple of the per-person price.

For example:

```javascript
"pricingInfo": {
   "type": "PER_PERSON",
   "ageBands": [
      {
        "ageBand": "CHILD",
        "startAge": 3,
        "endAge": 12,
        "minTravelersPerBooking": 0,
        "maxTravelersPerBooking": 6
      },
      {
        "ageBand": "ADULT",
        "startAge": 13,
        "endAge": 99,
        "minTravelersPerBooking": 2,
        "maxTravelersPerBooking": 6
      }
   ]
},
```

## Per-unit pricing

If the product has *unit* pricing, then the total price of the booking will depend on the number of units (groups) and types of unit that ideally accommodate the participant mix. Additionally, the `pricingInfo` object in the response will specify the type of unit in the `unitType` field; e.g., in this case, `"VEHICLE"`:

```javascript
"pricingInfo": {
  "type": "UNIT",
  "ageBands": [
    {
       "ageBand": "TRAVELER",
       "startAge": 0,
       "endAge": 99,
       "minTravelersPerBooking": 1,
       "maxTravelersPerBooking": 8
    }
  ],
  "unitType": "VEHICLE"
}
``` 

The `unitType` will be one of:


| `unitType`  | Example product | Meaning |
|-------------|-----------------|---------|
| `"GROUP"` | [10847P42](https://www.viator.com/tours/Berlin/In-Search-of-Jewish-Berlin-3-hour-Private-History-Tour-with-a-Scholar-Guide/d488-10847P42) | **Per-group** pricing – the unit price is calculated according to the number of groups the specified passenger mix will fit into rather than the exact number of participants. `minTravelersPerBooking` and `maxTravelersPerBooking` must be considered as these fields relate to the available group sizes. |
| `"ROOM"` | [16621P2](https://www.viator.com/tours/Maharashtra/Stay-package-with-amusement-park-for-2pax-for-2-nights/d25170-16621P2) | **Per-room** pricing relates the room price, which depends on the number of participants making the booking. |
| `"PACKAGE"` | [186385P1](https://www.viator.com/tours/Slovenia/VIP-wellness-to-rent/d734-186385P1) | **Per-package** pricing refers to products that are sold as part of a package; for example a family package stipulating a passenger mix of two adults and two children. |
| `"VEHICLE"` | [10175P10](https://www.viator.com/tours/Cannes/Private-Departure-Transfer-from-Cannes-to-Nice-Airport/d786-10175P10) | **Per-vehicle** pricing is calculated according to the number of vehicles required for the specified passenger mix rather than the exact number of participants. `minTravelersPerBooking` and `maxTravelersPerBooking` must be considered as these fields relate to the occupancy limitations for each vehicle. The minimum price will depend on the rate for a single vehicle. |
| `"BIKE"` | [153074P3](https://www.viator.com/tours/Adelaide/Adelaide-90-Minute-Pedicab-Tour-Scenic-Green-and-River-Experience/d376-16810P4) | **Per-bike** pricing – identical to "per vehicle", but refers specifically to vehicles that are bikes. |
| `"BOAT"` | [35157P2](https://www.viator.com/tours/British-Virgin-Islands/Private-Sunset-Sail/d809-35157P2) | **Per-boat** pricing – identical to "per vehicle", but refers specifically to vehicles that are boats. |
| `"AIRCRAFT"` | [14876P5](https://www.viator.com/tours/Niagara-Falls-and-Around/Air-Taxi-Tour-from-Niagara-to-Toronto-including-Ground-Transport-from-Niagara-Hotels/d773-14876P5) | **Per-aircraft** pricing – identical to "per vehicle", but refers specifically to vehicles that are aircraft. |  
