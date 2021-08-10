# API versioning strategy

In order to ensure the predictability of the behavior of this software, we have implemented a versioning strategy to handle updates to the API contract, as well as a mechanism to specify which version of the API you wish to access.

When we release a new version of this API, partners have the option of continuing to use the existing version or migrating to the updated version when they are ready.

  - **Note**: The version of this API that this document pertains to is that indicated in the title of this document.

The approach to versioning for this API is as follows:

## Global versioning

- Version numbers are global across all endpoints. When a new version of this API is released, **all** endpoints will increment to the latest version. Viator does not support a heterogenous implementation with calls being made to different endpoints with different version numbers.

## Version release and deprecation

- New versions of this software are released on an ad-hoc basis. Breaking changes will always result in a version increment.
- Viator will inform partners about all version releases, including details about the new features available and any breaking changes that have been introduced.
- Once a version of the API is deprecated, you will have a minimum of 12 months from the date of receiving notice of the change in which to modify your systems. Requests made to deprecated endpoint versions may result in a **400 Bad Request** response after 12 months.

## Release candidates

- Release candidate (RC) versions of this API may be made available to allow partners to preview changes in a non-production (sandbox) environment. When available, RC versions will be announced in this documentation, however these will not be subject to version control and breaking changes may be introduced prior to official release.

## Release criteria

### Backward-compatible changes

The following types of change are considered backward-compatible, and will not result in a version release when introduced. Therefore, partners must ensure their implementation can handle such changes gracefully. 

These changes comprise:

  - Introduction of new API endpoints
  - Addition of any properties to an endpoint's response schema
  - Addition of non-required properties to an endpoint's request schema
  - Unexpected receipt of an HTTP redirect response code (**301 Moved Permanently** or **302 Found**)
  - Addition of new HTTP methods (POST, GET, PUT, PATCH, DELETE, etc.)
  - Addition of new key values to existing fields that represent a set, insofar as no operating logic is likely to be affected

### Breaking changes

The following types of change are considered breaking changes and will result in the release of an incremented version of this software:

 - Addition of required properties to an endpoint's request schema
 - Removal of required properties from an endpoint's request or response schemata
 - Changing the data-type or format (e.g., date format) of an existing field in an endpoint's request or response schemata
 - Adding, removing or changing the meaning of the HTTP status codes in an endpoint's response
 - Removing or modifying the **Content-Type** on existing endpoints
 - Modifying or removing field key values in a set
 - Modifying the **operationId** of an endpoint

## Version specification mechanism

It is mandatory to specify which version of the API you wish to use via the **'Accept' header parameter** in the **request** to each API endpoint; e.g.,  `Accept: application/json;version=2.0`. Omitting the version parameter will result in a **400 Bad Request** response.

**Example valid request**:

```bash
curl --location --request GET 'https://api.sandbox.viator.com/partner/products/modified-since?cursor=MTU5MjM1NTM0MXwxMTM5ODZQNA==' \
--header 'exp-api-key: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx' \
--header 'Accept-Language: en-US' \
--header 'Accept: application/json;version=2.0'
```

**Example error response**:

```javascript
{
  "code":"INVALID_HEADER_VALUE",
  "message":"Accept header  is missing or has invalid version information",
  "timestamp":"2020-09-02T03:43:23.303946Z",
  "trackingId":"3D45567E:2D25_0A5D03AB:01BB_5F4F14A5_394639:3DB7"
}
```