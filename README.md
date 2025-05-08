# 🧭 Managing Gateways in Azure API Management (APIM)

This guide illustrates how to **add** and **remove** gateways from an API hosted in Azure API Management using REST API calls and Azure Active Directory (AAD) authentication.

It uses the default *echo* API that is installed in APIM on first provisioning.

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
