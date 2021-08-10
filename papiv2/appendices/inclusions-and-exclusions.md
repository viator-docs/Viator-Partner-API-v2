# Inclusions & exclusions

The `inclusions` and `exclusions` arrays are returned for active products in the response from the [product content endpoints](#section/Key-concepts/Content-ingestion-endpoints). The array items for both `inclusions` and `exclusions` are objects defined by the same schema, `InclusionExclusionItem`.

The following table describes the mapping of the possible combinations of `category`, `categoryDescription`, `type`, and `typeDescription` elements of the object in the response that you may encounter. 

<table>
<tr>
    <th><code>category</code></th>
    <th><code>categoryDescription</code></th>
    <th><code>type</code></th>
    <th><code>typeDescription</code></th>

</tr>
<tr>
    <td rowspan='7' colspan='1'><code>"FEES_AND_TAXES"</code></td>
    <td rowspan='7' colspan='1'>Fees</td>
    <td><code>"AIRPORT_AND_DEPARTURE_TAX"</code></td>
    <td>Airport/Departure Tax</td>
</tr>
<tr>
    <td><code>"ALL_FEES_AND_TAXES"</code></td>
    <td>All Fees and Taxes</td>
</tr>
<tr>
    <td><code>"FUEL_SURCHARGE"</code></td>
    <td>Fuel surcharge</td>
</tr>
<tr>
    <td><code>"GRATUITIES"</code></td>
    <td>Gratuities</td>
</tr>
<tr>
    <td><code>"GST"</code></td>
    <td>GST (Goods and Services Tax)</td>
</tr>
<tr>
    <td><code>"LANDING_AND_FACILITY_FEES"</code></td>
    <td>Landing and facility fees</td>
</tr>
<tr>
    <td><code>"PARKING_FEES"</code></td>
    <td>Parking Fees</td>
</tr>
<tr>
    <td rowspan='10' colspan='1' ><code>"FOOD_AND_DRINK"</code></td>
    <td rowspan='10' colspan='1' >Food and drink</td>
    <td><code>"ALCOHOLIC_BEVRAGES"</code><br /><code>"ALCOHOLIC_BEVERAGES"</code></td>
    <td>Alcoholic Beverages</td>
</tr>
<tr>
    <td><code>"BOTTLED_WATER"</code></td>
    <td>Bottled water</td>
</tr>
<tr>
    <td><code>"BREAKFAST"</code></td>
    <td>Breakfast</td>
</tr>
<tr>
    <td><code>"BRUNCH"</code></td>
    <td>Brunch</td>
</tr>
<tr>
    <td><code>"COFFEE_AND_TEA"</code></td>
    <td>Coffee and/or Tea</td>
</tr>
<tr>
    <td><code>"DINNER"</code></td>
    <td>Dinner</td>
</tr>
<tr>
    <td><code>"LUNCH"</code></td>
    <td>Lunch</td>
</tr>
<tr>
    <td><code>"REFRESHMENTS"</code></td>
    <td>Refreshments</td>
</tr>
<tr>
    <td><code>"SNACKS"</code></td>
    <td>Snacks</td>
</tr>
<tr>
    <td><code>"SODA_POP"</code></td>
    <td>Soda/Pop</td>
</tr>
<tr>
    <td rowspan='2' colspan='1' ><code>"SOUVENIRS"</code></td>
    <td rowspan='2' colspan='1' >Souvenirs</td>
    <td><code>"CERTIFICATE"</code></td>
    <td>Certificate</td>
</tr>
<tr>
    <td><code>"RECIPE_BOOKLET"</code></td>
    <td>Recipe booklet</td>
</tr>
<tr>
    <td rowspan='4' colspan='1' ><code>"TRANSPORT_AMENITIES"</code></td>
    <td rowspan='4' colspan='1' >Transportation amenities</td>
    <td><code>"AIR_CONDITIONED_VEHICLE"</code></td>
    <td>Air-conditioned vehicle</td>
</tr>
<tr>
    <td><code>"PRIVATE_TRANSPORTATION"</code></td>
    <td>Private transportation</td>
</tr>
<tr>
    <td><code>"RESTROOM_ON_BOARD"</code></td>
    <td>Restroom on board</td>
</tr>
<tr>
    <td><code>"WIFI_ONBOARD"</code></td>
    <td>WiFi on board</td>
</tr>
<tr>
    <td rowspan='8' colspan='1' ><code>"EQUIPMENT"</code></td>
    <td rowspan='8' colspan='1' >Use of Equipment</td>
    <td><code>"ALL_INGREDIENTS"</code></td>
    <td>All ingredients</td>
</tr>
<tr>
    <td><code>"USE_OF_OTHER_EQUIPMENT"</code></td>
    <td>Other</td>
</tr>
<tr>
    <td><code>"USE_OF_BICYCLE"</code></td>
    <td>Use of bicycle</td>
</tr>
<tr>
    <td><code>"USE_OF_COOKING_UTENSILS"</code></td>
    <td>Use of cooking utensils</td>
</tr>
<tr>
    <td><code>"USE_OF_SCUBA_EQUIPMENT"</code></td>
    <td>Use of SCUBA equipment</td>
</tr>
<tr>
    <td><code>"USE_OF_SEGWAY"</code></td>
    <td>Use of Segway</td>
</tr>
<tr>
    <td><code>"USE_OF_SNORKELING_EQUIPMENT"</code></td>
    <td>Use of Snorkelling equipment</td>
</tr>
<tr>
    <td><code>"USE_OF_TRIKKE"</code></td>
    <td>Use of Trikke</td>
</tr>
<tr>
    <td><code>"OTHER"</code></td>
    <td>Other</td>
    <td><code>"OTHER"</code></td>
    <td>Other</td>
</tr>
</table>