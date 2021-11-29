# Booking questions

**Note**: This section applies to **merchant partners** only.

The booking-questions functionality of this API allows vital information specified by the supplier about the individual travelers or the group as a whole to be sent to the supplier as part of the request when making a booking using the [/bookings/book](../../../openapi/reference/operation/bookingsBook) endpoint.

The booking questions available for a product are specified in the `bookingQuestions` array in the response from any of the [product content endpoints](../key-concepts/content-ingestion-endpoints) for that product. For example, for the product [10212P2 (Taste of Miami Helicopter Tour)](https://www.viator.com/tours/Miami/Taste-of-Miami-Helicopter-Tour/d662-10212P2), the `bookingQuestions` array is as follows:

```javascript
"bookingQuestions": [
    "FULL_NAMES_LAST",
    "SPECIAL_REQUIREMENTS",
    "FULL_NAMES_FIRST",
    "WEIGHT",
    "AGEBAND"
],
```
Each key string (`"FULL_NAMES_LAST"`, etc.) identifies a booking question, details about which can be found in the response from the [/products/booking-questions](../../../openapi/reference/operation/productsBookingQuestions) endpoint.

Relevant booking question details in example response snippet from [/products/booking-questions](../../../openapi/reference/operation/productsBookingQuestions):

```javascript
"bookingQuestions": [
    {
        "legacyBookingQuestionId": 23,
        "id": "WEIGHT",
        "type": "NUMBER_AND_UNIT",
        "group": "PER_TRAVELER",
        "label": "Traveler weight in pounds or kilograms (required for safety reasons)",
        "hint": "E.g. 130lbs, 60kg",
        "units": [
            "kg",
            "lbs"
        ],
        "required": "MANDATORY"
    },
    {
        "legacyBookingQuestionId": 24,
        "id": "FULL_NAMES_FIRST",
        "type": "STRING",
        "group": "PER_TRAVELER",
        "label": "First name",
        "required": "MANDATORY"
    },
    {
        "legacyBookingQuestionId": 25,
        "id": "FULL_NAMES_LAST",
        "type": "STRING",
        "group": "PER_TRAVELER",
        "label": "Last name",
        "required": "MANDATORY"
    },
    {
        "legacyBookingQuestionId": 30,
        "id": "AGEBAND",
        "type": "STRING",
        "group": "PER_TRAVELER",
        "label": "Age band",
        "allowedAnswers": [
            "ADULT",
            "SENIOR",
            "YOUTH",
            "CHILD",
            "INFANT",
            "TRAVELER"
        ],
        "required": "MANDATORY"
    },
    {
        "legacyBookingQuestionId": 27,
        "id": "SPECIAL_REQUIREMENTS",
        "type": "STRING",
        "group": "PER_BOOKING",
        "label": "Special requirements",
        "required": "OPTIONAL"
        },
    ...
]
```

The `required` field indicates whether an answer to the booking question must be provided in the booking request. It is necessary to provide an answer to all specified booking questions for which `required` is `MANDATORY`. 

Additionally, the `group` field indicates whether an answer to the booking question must be answered for each traveler (`"PER_TRAVELER"`) or for the booked group as a whole (`"PER_BOOKING"`).

In this case:

| Booking question id | required | group |
|---------------------|----------|-------|
| `"WEIGHT"` | `"MANDATORY"` | `"PER_TRAVELER"` |
| `"FULL_NAMES_FIRST"` | `"MANDATORY"` | `"PER_TRAVELER"` |
| `"FULL_NAMES_LAST"` | `"MANDATORY"` | `"PER_TRAVELER"` |
| `"AGEBAND"` | `"MANDATORY"` | `"PER_TRAVELER"` |
| `"SPECIAL_REQUIREMENTS"` | `"OPTIONAL"` | `"PER_BOOKING"` |

Therefore, to book this product, you must provide an answer to the `"WEIGHT"`, `"FULL_NAMES_FIRST"`,`"FULL_NAMES_LAST"` and `"AGEBAND"` questions for each traveler, and, optionally (if specified by the user at the time of booking) `"SPECIAL_REQUIREMENTS"` for the booked group.

Answers to the booking questions are sent in the `bookingQuestionAnswers[]` array in the request to to [/bookings/book](../../../openapi/reference/operation/bookingsBook). The following example shows a valid and complete set of booking-question answers for this product.

| Traveler number | First name | Last name | age band |
|-----------------|------------|-----------|----------|
| 1 | John | Smith | ADULT |
| 2 | Mary | Jones | ADULT | 

```javascript
"bookingQuestionAnswers": [  
    {
      "question": "AGEBAND",
      "answer": "ADULT",
      "travelerNum": 1
    },
    {
      "question": "FULL_NAMES_LAST",
      "answer": "Smith",
      "travelerNum": 1
    },
    {
      "question": "FULL_NAMES_FIRST",
      "answer": "John",
      "travelerNum": 1
    },
    {
      "question": "AGEBAND",
      "answer": "ADULT",
      "travelerNum": 2
    },
    {
      "question": "FULL_NAMES_LAST",
      "answer": "Jones",
      "travelerNum": 2
    },
    {
      "question": "FULL_NAMES_FIRST",
      "answer": "Mary",
      "travelerNum": 2
    },
    {
      "question": "SPECIAL_REQUIREMENTS",
      "answer": "NO"
    }
]
```

## When to use `travelerNum`

Please note that `travelerNum` indicates which traveler the answer pertains to. All `PER_TRAVELER` questions must be answered for all travelers in the booking. `travelerNum` **must** be omitted for `PER_BOOKING` booking questions.

## Booking questions with units

Some booking questions require you to specify the unit type when providing an answer to the questions.

**Example: 'HEIGHT' booking question**


Definition of question from [/products/booking-questions](../../../openapi/reference/operation/productsBookingQuestions):
```javascript
{
    "legacyBookingQuestionId": 2,
    "id": "HEIGHT",
    "type": "NUMBER_AND_UNIT",
    "group": "PER_TRAVELER",
    "label": "Traveler height in feet or centimeters (required for safety reasons)",
    "hint": "E.g. 5'2\", 158cm",
    "units": [
        "cm",
        "ft"
    ],
    "required": "MANDATORY"
},
```

A valid answer to this booking question for a traveler who is 193 centimetres tall is as follows:

```javascript
"bookingQuestionAnswers": [  
    {
      "question": "HEIGHT",
      "answer": "193",
      "unit": "cm",
      "travelerNum": 1
    }
]
```

**Example: 'WEIGHT' booking question**

Definition of question from [/products/booking-questions](../../../openapi/reference/operation/productsBookingQuestions):

```javascript
{
  "legacyBookingQuestionId": 23,
  "id": "WEIGHT",
  "type": "NUMBER_AND_UNIT",
  "group": "PER_TRAVELER",
  "label": "Traveler weight in pounds or kilograms (required for safety reasons)",
  "units": [
      "kg",
      "lbs"
  ],
  "required": "MANDATORY",
  "maxLength": 50
},
```

A valid answer to this booking question for a traveler who weighs 69 kilograms would be as follows:

```javascript
"bookingQuestionAnswers": [  
    {
      "question": "WEIGHT",
      "answer": "69",
      "unit": "kg",
      "travelerNum": 1
    }
]
```


## Booking questions with allowed answers

Some booking questions must be answered in a specific way. These questions include an `allowedAnswers` array containing the set of valid answers to the question.

Example booking question with allowed answers:

```javascript
{
    "legacyBookingQuestionId": 31,
    "id": "TRANSFER_ARRIVAL_MODE",
    "type": "STRING",
    "group": "PER_BOOKING",
    "label": "Arrival mode",
    "allowedAnswers": [
        "AIR",
        "RAIL",
        "SEA"
    ],
    "required": "MANDATORY"
}
```

A valid answer to this booking question would be:

```javascript
{
    "question": "TRANSFER_ARRIVAL_MODE",
    "answer": "AIR"
},
```

## Conditional booking questions

There are some booking questions that may or may not have to be answered, depending on the presence of – or answer to – another booking question. These questions are 'conditional', as indicated by the `required` field containing `"CONDITIONAL"`; e.g.:

```javascript
{
    "legacyBookingQuestionId": 7,
    "id": "TRANSFER_AIR_ARRIVAL_AIRLINE",
    "type": "STRING",
    "group": "PER_BOOKING",
    "label": "Name of arrival airline",
    "hint": "E.g. United Airlines",
    "required": "CONDITIONAL",
    "maxLength": 255
},
```

So, when is an answer to a `"CONDITIONAL"` booking question required?

At present, the logic cannot be inferred from the response from the [/products/booking-questions](../../../openapi/reference/operation/productsBookingQuestions) endpoint, but it can be explained here. You will need to hard-code this logic into your implementation in order to correctly present the required booking questions to the user.

The logic runs as follows:

| For this booking question | if the user's answer is | these questions must also be answered |
|--------------------|----------------|-------------------------------|
| `"TRANSFER_ARRIVAL_MODE"` | `"AIR"` | `"TRANSFER_AIR_ARRIVAL_AIRLINE"`<br />`"TRANSFER_AIR_ARRIVAL_FLIGHT_NO"`<br />`"TRANSFER_ARRIVAL_TIME"`<br />`"TRANSFER_ARRIVAL_DROP_OFF"` (see condition 1)<br />`"PICKUP_POINT"` (see condition 1) |
| `"TRANSFER_ARRIVAL_MODE"` | `"RAIL"` | `"TRANSFER_RAIL_ARRIVAL_LINE"`<br />`"TRANSFER_RAIL_ARRIVAL_STATION"`<br />`"TRANSFER_ARRIVAL_TIME"`<br />`"TRANSFER_ARRIVAL_DROP_OFF"` |
| `"TRANSFER_ARRIVAL_MODE"` | `"SEA"` | `"TRANSFER_PORT_ARRIVAL_TIME"`<br />`"TRANSFER_PORT_CRUISE_SHIP"`<br />`"TRANSFER_ARRIVAL_DROP_OFF"` (see condition 2) <br />`"PICKUP_POINT"` (see condition 2)|
| `"TRANSFER_ARRIVAL_MODE"` | `"OTHER"` | `"PICKUP_POINT"` (see condition 3) |
| `"TRANSFER_DEPARTURE_MODE"` | `"AIR"` | `"TRANSFER_AIR_DEPARTURE_AIRLINE"`<br />`"TRANSFER_AIR_DEPARTURE_FLIGHT_NO"`<br />`"TRANSFER_DEPARTURE_DATE"`<br />`"TRANSFER_DEPARTURE_TIME"`<br />`"TRANSFER_DEPARTURE_PICKUP"` |
| `"TRANSFER_DEPARTURE_MODE"` | `"RAIL"` | `"TRANSFER_RAIL_DEPARTURE_LINE"`<br />`"TRANSFER_RAIL_DEPARTURE_STATION"`<br />`"TRANSFER_DEPARTURE_DATE"`<br />`"TRANSFER_DEPARTURE_TIME"`<br />`"TRANSFER_DEPARTURE_PICKUP"` |
| `"TRANSFER_DEPARTURE_MODE"` | `"SEA"` | `"TRANSFER_PORT_CRUISE_SHIP"`<br />`"TRANSFER_DEPARTURE_DATE"`<br />`"TRANSFER_PORT_DEPARTURE_TIME"`<br />`"TRANSFER_DEPARTURE_PICKUP"`|
| `"TRANSFER_DEPARTURE_MODE"` | `"OTHER"` | n/a |



**Conditions**:

1. Rule applies only if `logistics.travelerPickup.locations[]` includes an item with `pickupType`: `"AIRPORT"`; **or**, if `logistics.travelerPickup.allowCustomTravelerPickup` is **true**.
2. Rule applies only if `logistics.travelerPickup.locations[]` includes an item with `pickupType`: `"PORT"`; **or**, if `logistics.travelerPickup.allowCustomTravelerPickup` is **true**.
3. Rule applies only if `logistics.travelerPickup.locations[]` includes an item with `pickupType`: `"HOTEL"` or `pickupType`: `"LOCATION"`; **or**, if `logistics.travelerPickup.allowCustomTravelerPickup` is **true**.


**Extra notes**:

- All `"CONDITIONAL"` booking questions that depend on the answer given to either `"TRANSFER_ARRIVAL_MODE"` or `"TRANSFER_DEPARTURE_MODE"` (i.e., those questions in the right-hand column in the table above) should be considered `"MANDATORY"` if they are stipulated in the `"bookingQuestions"` array of the product content response, and the respective transfer mode question **is not** stipulated. That is to say, for example, if `"TRANSFER_AIR_ARRIVAL_AIRLINE"` is present, but `"TRANSFER_ARRIVAL_MODE"` is absent from `"bookingQuestions"`, then `"TRANSFER_AIR_ARRIVAL_AIRLINE"` should be considered `"MANDATORY"`.
- The `"TRANSFER_PORT_CRUISE_SHIP"` question is required to be answered when the customer's response to either `"TRANSFER_ARRIVAL_MODE"` or `"TRANSFER_DEPARTURE_MODE"` is `"SEA"`; however, this question must only be answered **once** per booking. I.e., the answer for `"TRANSFER_PORT_CRUISE_SHIP"` pertains to both questions. There is no provision for the customer to specify different cruise ships for arrival and departure.

## Pickup-point question

The booking question that requests the location for pickup is as follows:

```javascript
{
    "legacyBookingQuestionId": 6,
    "id": "PICKUP_POINT",
    "type": "LOCATION_REF_OR_FREE_TEXT",
    "group": "PER_BOOKING",
    "label": "Hotel pickup",
    "hint": "E.g. 1234 Cedar Way, Brooklyn NY 00123",
    "units": [
        "LOCATION_REFERENCE",
        "FREETEXT"
    ],
    "required": "CONDITIONAL",
    "maxLength": 1000
}
```

As you can see, this question can be answered using a location reference or 'freetext' (i.e., a written address in this context). Whether freetext is allowed for this answer depends on the value of `logistics.travelerPickup.allowCustomTravelerPickup` in the response from any of the [product content endpoints](../key-concepts/content-ingestion-endpoints). If `true`, freetext is allowed. Otherwise, a location reference – in particular, one of the location references included in the `logistics.travelerPickup.locations[]` array – must be supplied.

Example `"FREETEXT"` answer:

```javascript
"bookingQuestionAnswers": [  
    {
      "question": "PICKUP_POINT",
      "answer": "1234 Cedar Way, Brooklyn NY 00123",
      "unit": "FREETEXT"
    }
]
```

Example `"LOCATION_REFERENCE"` answer:

```javascript
"bookingQuestionAnswers": [  
    {
      "question": "PICKUP_POINT",
      "answer": "LOC-6cb31b00-1fb4-4218-9b50-63f66531d735",
      "unit": "LOCATION_REFERENCE"
    }
]
```

### Special pickup-point location references

The `logistics.travelerPickup.locations[]` array in the response from the product content endpoints contains the location references for all available pickup locations for a product.

As well as standard location reference codes; e.g., "LOC-6cb31b00-1fb4-4218-9b50-63f66531d735", there are two special codes that specify **instructions** rather than locations. 

These are:

- `"MEET_AT_DEPARTURE_POINT"`
- `"CONTACT_SUPPLIER_LATER"`

When selecting available pickup-points, our suppliers also have the option of specifying one or both of these pseudo-locations. These instruct the customer to either meet at one of the locations specified in `logistics.start[]`, or to contact the supplier later to arrange a pickup-point, respectively.

When building a list of available pickup locations for the customer to select at the time of booking, the descriptive text for these locations can be used. This is available from the [/locations/bulk](../../../openapi/reference/operation/locationsBulk) endpoint in the `name` field, as follows:

```javascript
{
    "locations": [
        {
            "provider": "TRIPADVISOR",
            "reference": "CONTACT_SUPPLIER_LATER",
            "name": "I will contact the supplier later"
        },
        {
            "provider": "TRIPADVISOR",
            "reference": "MEET_AT_DEPARTURE_POINT",
            "name": "I will meet at the departure point"
        }
    ]
}
```

Or, you can provide a selection button in your UI, as we have done on the [viator.com](https://viator.com) site:

<figure>
    <img src="img/pick-up-point.png" alt="I will select my pickup location later button"/>
    <figcaption>"I will select my pickup location later" example from viator.com</figcaption>
</figure>

This button appears when the `"CONTACT_SUPPLIER_LATER"` location reference is included in the `logistics.travelerPickup.locations[]` array.

## How to determine if a product option supports pickup

When the value of `logistics.travelerPickup.pickupOptionType` in the product content response is `"PICKUP_AND_MEET_AT_START_POINT"`, it means that the product includes both 'hotel pickup' and 'meet at the departure point' variants in its product options; e.g., one product option may be for hotel pickup while another is for meeting at the start/departure point.

To determine which product options offer what, it is necessary to inspect each product option's details in the `productOptions[]` array in the product content response.

If pickup is included for a product option, the phrase `Pickup included` will be present in the `productOptions[].description` field, as well as any other comments provided by the supplier. For example, the following two product options from product [8374P24](https://www.viator.com/tours/Bangkok/Train-Market-and-Damnoensaduak-Floating-Market-with-small-group/d343-8374P24) both offer pickup:

```javascript
"productOptions": [
  {
      "productOptionCode": "TG5",
      "description": "Private tour: Private tour only your group<br/>Pickup included",
      "title": "Private tour",
      "languageGuides": [
          {
              "type": "GUIDE",
              "language": "en",
              "legacyGuide": "en/SERVICE_GUIDE"
          }
      ]
  },
  {
      "productOptionCode": "TG4",
      "description": "Pickup included",
      "title": "Hotel pick up included",
      "languageGuides": [
          {
              "type": "GUIDE",
              "language": "en",
              "legacyGuide": "en/SERVICE_GUIDE"
          }
      ]
  },
  ...
]
```

When booking a product option with pickup included, such as this, you must collect a pickup-point from the user that corresponds to one of the entries in the `logistics.travelerPickup.locations[]` array in the product content response.

If the `productOptions[].description` field **does not** contain the phrase `Pickup included`, this indicates that this product option does not include pickup and should be considered to follow a `"MEET_AT_DEPARTURE_POINT"` arrangement. 

For example, the following product option (also from product [8374P24](https://www.viator.com/tours/Bangkok/Train-Market-and-Damnoensaduak-Floating-Market-with-small-group/d343-8374P24)) requires meeting at the departure point and does not include pickup:

```javascript
"productOptions": [
  {
      "productOptionCode": "TG3",
      "description": "",
      "title": "Meeting point",
      "languageGuides": [
          {
              "type": "GUIDE",
              "language": "en",
              "legacyGuide": "en/SERVICE_GUIDE"
          }
      ]
  },
  ...

```

For product options such as these, there is no need to collect the pickup-point from the user. When sending the booking request using the [/bookings/book](../../../openapi/reference/operation/bookingsBook) endpoint, simply use `"MEET_AT_DEPARTURE_POINT"` as the answer.


```javascript
"bookingQuestionAnswers": [  
    {
      "question": "PICKUP_POINT",
      "answer": "MEET_AT_DEPARTURE_POINT",
      "unit": "LOCATION_REFERENCE"
    }
]
```


## Age-bands

If `"AGEBAND"` is specified as a booking question in the `bookingQuestions` array in the product content response, you must supply the age-band for each traveler in `bookingQuestionAnswers` when making the booking using [/bookings/book](../../../openapi/reference/operation/bookingsBook). 

Furthermore, the answer(s) to the `"AGEBAND"` booking question that you submit in the request to [/bookings/book](../../../openapi/reference/operation/bookingsBook) must match the age-bands given in the `paxMix` element in the request to [/bookings/hold](../../../openapi/reference/operation/bookingsHold). Otherwise, the booking server will reject the booking on account of the booking and booking-hold not matching.

The valid age-bands for the product are given in the `pricingInfo` array in the product content response; e.g.:

```javascript
"pricingInfo": {
  "type": "PER_PERSON",
  "ageBands": [
    {
      "ageBand": "INFANT",
      "startAge": 0,
      "endAge": 4,
      "minTravelersPerBooking": 0,
      "maxTravelersPerBooking": 9
    },
    {
      "ageBand": "CHILD",
      "startAge": 5,
      "endAge": 15,
      "minTravelersPerBooking": 0,
      "maxTravelersPerBooking": 9
    },
    {
      "ageBand": "ADULT",
      "startAge": 16,
      "endAge": 99,
      "minTravelersPerBooking": 0,
      "maxTravelersPerBooking": 9
    }
  ]
},
```

Note that if `bookingRequirements.requiresAdultForBooking` is `true` in the product content response, at least one traveler must have an `"ADULT"` or `"SENIOR"` age-band.

### Age-bands with unit pricing

In the case that the product uses unit pricing; i.e., pricing based on the number of groups of travelers, then the product will have a single available age-band: `"TRAVELER"`, as given in the `pricingInfo` array in the product content response; e.g.:

```javascript
"pricingInfo": {
  "type": "UNIT",
  "ageBands": [
    {
      "ageBand": "TRAVELER",
      "startAge": 0,
      "endAge": 99,
      "minTravelersPerBooking": 1,
      "maxTravelersPerBooking": 10
    }
  ],
  "unitType": "GROUP"
}
```

For such products, even if `"AGEBAND"` is specified as a booking question, the only applicable age-band is `"TRAVELER"`. However, you must still supply this value for the age-band booking question for each traveler when making the booking using [/bookings/book](../../../openapi/reference/operation/bookingsBook). Even if `bookingRequirements.requiresAdultForBooking` is `true` for such products, you must still supply the `"TRAVELER"` value.