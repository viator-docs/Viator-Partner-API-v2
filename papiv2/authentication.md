# Authentication

Access to the API is managed using an **API key** that is included as a **header parameter** to every call made to all API endpoints described in this document.

| Header parameter name | Example value |
|-----------------------|---------------|
| exp-api-key | bcac8986-4c33-4fa0-ad3f-75409487026c |

If you do not know the API key for your organization, please contact your business development account manager for these details.

Please note that language localization is now controlled on a per-call basis. Previously, localization settings were configured per API-key; whereas, under the present scheme, organizations have a **single API key**. 

Language localization is accomplished by specifying the desired language as a header parameter (`Accept-Language`). See [Accept-Language header](#section/Localization/Accept-Language-header-parameter) for available langage codes.