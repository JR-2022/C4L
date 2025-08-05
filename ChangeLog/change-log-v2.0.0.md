# Cloud4Log Basic – Change Log

# Release v2.0.0 (01.09.2025) – Breaking Changes

**Last updated:** 05 August 2025  
*Note: This is a preliminary release note and subject to change.*

---

## OAuth 2.1 and Keycloak Integration

With the upcoming release of **Cloud4Log Basic v2.0.0**, we are introducing **OAuth 2.1** as the new standard for authentication. As part of this update, **Keycloak** will replace the existing login system as the identity provider.

This change includes the **deprecation** of the current authentication endpoints. While these endpoints will remain temporarily available to support a smooth transition for API users, we strongly encourage all users to migrate to the new Keycloak-based authentication as soon as possible. The deprecated endpoints will be **removed in a future release**.

### Deprecated Authentication Endpoints

- `POST /authentication/login`  
- `POST /authentication/loginTwoFARequest`  
- `POST /authentication/logout`  

### Migration Process

All users will be required to authenticate through Keycloak moving forward. To support this migration, users will receive an email with a link to set a new password, which will be stored in Keycloak. You may choose to reuse your current password, but this is not required.

During the transition period, your existing password will still work with the deprecated authentication endpoints.

### New Authentication via Keycloak

> **To be announced**  
Details regarding the new authentication flow will be provided in a future update.
