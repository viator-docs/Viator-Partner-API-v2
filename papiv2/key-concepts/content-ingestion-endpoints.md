# Content ingestion endpoints

## Product content endpoints

You can retrieve the core product-content details from the following endpoints:

- [/products/{product-code}](../../../openapi/reference/operation/products): Retrieve content for a single product
- [/products/modified-since](../../../openapi/reference/operation/productsModifiedSince): Retrieve content for all products with filtering according to modification date
- [/products/bulk](../../../openapi/reference/operation/productsBulk): Retrieve content for all products specified in the request

## Availability schedules endpoints

You can retrieve the availability schedules for products using the following endpoints:

- [/availability/schedules/{product-code}](../../../openapi/reference/operation/availabilitySchedules): Retrieve availability schedules for a single product
- [/availability/schedules/modified-since](../../../openapi/reference/operation/availabilitySchedulesModifiedSince): Retrieve availability schedules for all products with filtering according to modification date
- [/availability/schedules/bulk](../../../openapi/reference/operation/availabilitySchedulesBulk): Retrieve availability schedules for all products specified in the request