# C4L- Cloud4Log
## Share delivery notes with partners via an API solution (Paperless)!
### Make delivery notes visible / usable in digital format in the cloud for partners of your logistics processes.
 
In this JSON API you will find REST methods for creating, editing and sharing delivery note information. Roles and rights can be managed via the admin tool and can be assigned to users per company, location or function-related. In the area of contract logistics, rights can be temporarily passed on to partner companies. 
Delivery bill data can be linked to each other through assignments at process level or delimited from each other at company level.

---

Here are the available API documentation versions:

- **[API Documentation v2](https://api.cloud4log.com/api-docs/)**  
  Stable version for existing integrations.

- **[API Documentation v3](https://api.cloud4log.com/api-docs-v3/)**  
  New version with **pagination support** and additional features.

- **[Combined API Documentation (v2 + v3)](https://api.cloud4log.com/api-docs-combined/)**  
  Useful if you need to work with both versions at the same time.

If you want to test the APIs through the Swagger-UI or Scalar-UI, you can authorize yourself in two ways:

1. **Legacy API Key** – Enter your API key directly.  
2. **Bearer Token (Keycloak Authentication)** –  
   - Obtain a token using the Keycloak login flow.  
   - **Remove the `Bearer ` prefix** before pasting it into the authorization field.  
   - Keep in mind: **Bearer tokens are valid for only 3 minutes**.

---

Solution overview:

- **[Solution Overview Diagram](https://viewer.diagrams.net/?tags=%7B%7D&highlight=0000ff&edit=_blank&layers=1&nav=1&title=C4l%20Systemüberblick.drawio#Uhttps%3A%2F%2Fdrive.google.com%2Fuc%3Fid%3D1ZgyjjVKgR4EoMwbgwfXX4AM-fBNV5M0A%26export%3Ddownload)**  
  High-level architecture diagram of the Cloud4Log system.

---

In the API, you can set up and use the following roles and functions:
 
### Organizational units (OU) (company or location).
1. consignor/ manufacturer
2. carrier / forwarder, contract logistics provider
3. consignee/ receiver, stationary trade
### Functions of a company or location:
- Outgoing goods (consignor)
- Carrier / contract logistics
- Logistician / Driver
- Incoming goods (consignee)
### Roles: 
* Admin Organizational Unit (OU)
* Admin Location
* User OU or location
* Driver / carrier
