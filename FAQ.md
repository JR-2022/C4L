# C4L- Cloud4Log FAQ
## Frequently Asked Questions
### How can the API be accessed?

### **Option 1: Access via Bearer Token (recommended method)**

1. Log in to the **frontend**. 
2. In the **Org Admin’s** Admin Panel, create a new **Keycloak Client** under **“API Clients”**. 
3. Select the **organization sites** for which the client should be valid. 

The type of client depends on the **use case**:

- **Public Client:** For third-party frontend applications.  
- **Confidential Client:** For server-side applications that can securely store a client secret and authenticate with the authorization server.  
  *(Flow: Authorization Code with Client Authentication)*  
- **M2M Client:** For machine-to-machine communication without user context.  
  *(Flow: Client Credentials)*  

With the created client, authentication can be performed on the Keycloak instance, and then communication with the API can be done using a Bearer Token.  

More information can be found in the documentation:  
- **[API Documentation: OAuth2](https://github.com/JR-2022/C4L/blob/main/OAuth2-Authentication.md)**  

### **Option 2: Access via Legacy API Key (deprecated method)**

Alternatively, a **Legacy API Key** can be generated: 

⚠️ **Note:** This method is only intended for existing integrations and will be **replaced by OAuth2 authentication from 01.11.2026**.

1. Log in as an **Org Admin** in the **Admin Panel**.  
2. Navigate to the **“Legacy API Keys”** section.  
3. Create a new **Legacy API Key** and select the desired **organization sites**.  

This **API Key** (also known as `sessionid` or `dlssessionid`) can also be used to send requests to the API.  

---
### How can I integrate my company's Keycloak SSO with the new authentication?
**[Integration of external Identity Provider (IDP)](https://github.com/JR-2022/C4L/blob/main/setup-external-idp.md)** 

---
### Where can I find information about the API?
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
   - Keep in mind: **Bearer tokens are valid for only 5 minutes**.

