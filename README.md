# 🧭 Managing Gateways in Azure API Management (APIM)

This guide illustrates how to **add** and **remove** gateways from an API hosted in Azure API Management using REST API calls and Azure Active Directory (AAD) authentication.

It uses the default *echo* API that is installed in APIM on first provisioning.

The REST API is [Gateway Api - Create Or Update](https://learn.microsoft.com/en-us/rest/api/apimanagement/gateway-api/create-or-update?view=rest-apimanagement-2024-05-01&tabs=HTTP)

---


## 🔁 Sending Echo Requests

Two sample requests demonstrate how to send a test request to an echo API. This is when running the gateway as a Docker container locally.

- **Local Gateway Request**:
```http
GET http://localhost:80/echo/resource?param1=sample
Ocp-Apim-Subscription-Key: <subscription-key>
```

- **Managed Gateway Request**:
```http
GET http://jjtest2.azure-api.net/echo/resource?param1=sample
Ocp-Apim-Subscription-Key: <subscription-key>
```

## 🔐 Authentication: Get an Access Token
To interact with the Azure Management API, an OAuth 2.0 token is required. This is obtained using a service principal. The service principal must have sufficient privilages to manage the API Management instance and so should have the correct role assignments prior to getting the access token:

**POST to Azure AD Token Endpoint**:
```http
POST https://login.microsoftonline.com/{{tenantId}}/oauth2/v2.0/token
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials
&client_id={{clientId}}
&client_secret={{clientSecret}}
&scope=https%3A%2F%2Fmanagement.azure.com%2F.default
```

Extract the Access Token:
```http
@access_token = {{getToken.response.body.access_token}}
```

## ➕Add a Gateway to an API
To associate a gateway with an API requires a PUT request

```http
PUT https://management.azure.com/subscriptions/{{subscriptionId}}/resourceGroups/{{resourceGroupName}}/providers/Microsoft.ApiManagement/service/{{serviceName}}/gateways/{{gatewayId}}/apis/{{apiId}}?api-version=2024-05-01
Content-Type: application/json
Authorization: Bearer {{access_token}}

{}
```

This binds the specified gatewayId (e.g., jjgateway) to the API (echo-api).

## ➖ Remove a Gateway from an API
To disassociate a gateway (e.g., the managed gateway) from the API:

```http
DELETE https://management.azure.com/subscriptions/{{subscriptionId}}/resourceGroups/{{resourceGroupName}}/providers/Microsoft.ApiManagement/service/{{serviceName}}/gateways/{{managedGatewayId}}/apis/{{apiId}}?api-version=2024-05-01
Content-Type: application/json
Authorization: Bearer {{access_token}}
```

This removes the link between the API and the specified gateway.

## ✅ Summary
Echo requests test connectivity through different gateways.
AAD token is required for secure API management operations.
PUT adds a gateway to an API.
DELETE removes a gateway from an API.
