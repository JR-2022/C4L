# Cloud4Log Basic – Change Log

# Release v2.0.0 – Breaking Changes

**Last updated:** 22 July 2025  
*Note: This is a preliminary release note and subject to change.*

---

## Deprecated Properties in the Following Endpoints

### `POST /authentication/login`

We decided to not move forward with the cookie-based authentication flow.  
The `cookie` parameter will be removed.  
The `dlssessionid` header is once again the standard for authentication via this endpoint.

---

### `POST /organization-sites/{organizationSiteKey}/consignor/delivery-note-bundles-checkout`

**Request schema (`DeliveryNoteBundlesCheckout`)**:  
The `carrierSignature` property is deprecated and will be removed.

---

### `GET /delivery-notes/{deliveryNoteKey}`

**Response schema (`SingleDeliveryNote`)**:  
The `loadCarriers` property is deprecated and will be removed.

---

### `POST /organization-sites/{organizationSiteKey}/events`

**Request schema (`TriggerEvent`)**:  
The `checkinKey` property is deprecated and will be removed.  
The `checkinKeys` property is now required.

**Response schema (`TriggerEventResponse`)**:  
The `eventKey` property is deprecated and will be removed.  
The `eventKeys` property is now required.

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
