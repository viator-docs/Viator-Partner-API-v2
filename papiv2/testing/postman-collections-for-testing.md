# Postman collections for testing

To facilitate your testing of the API's functionality in the sandbox environment, please use one of the following [Postman](https://www.getpostman.com/) collections, depending on which type of partner you are:

  - [Viator Merchant API Postman collection](Viator-Merchant-API-v2.postman_collection.json)
  - [Viator Basic-access Affiliate API Postman collection](Viator-Basic-Access-Affiliate-API-v2.postman_collection.json)
  - [Viator Full-access Affiliate API Postman collection](Viator-Affiliate-API-v2.postman_collection.json)

## Set up the `your-api-key` environment variable

As all services in this API require that your API-key be passed as a header parameter in every request made to this API, this value has been set to reference a Postman environment variable called `your-api-key`.

Once you import the collection into Postman, [set this variable](https://learning.getpostman.com/docs/postman/environments_and_globals/variables#defining-collection-variables) to your organization's API-key and save the collection.