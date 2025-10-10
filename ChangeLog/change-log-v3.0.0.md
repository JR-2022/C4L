# Cloud4Log Basic – Change Log

# Release v3.0.0 (03.08.2026) – Breaking Changes

**Last updated:** 05 August 2025
*Note: This is a preliminary release note and subject to change.*

## Deprecated Endpoints

### Authentication Endpoints

- `POST /authentication/login`  
- `POST /authentication/loginTwoFARequest`  
- `POST /authentication/logout`  
These endpoints will be removed. Use the new Keycloak-based OAuth2.1 authentication system.

### Consignee Attachments (Receipts) Endpoints

API v2 endpoint is deprecated:
- `GET /organization-sites/{organizationSiteKey}/consignee/delivery-notes/{deliveryNoteKey}/receipts/{attachmentKey}` 

Use the new API v3 endpoint:
- `GET /organization-sites/{organizationSiteKey}/consignee/delivery-notes/{deliveryNoteKey}/receipts/{attachmentKey}/file`

---

## Deprecated Properties in the Following Endpoints

### `POST /organization-sites/{organizationSiteKey}/consignor/delivery-note-bundles-checkout`

**Request schema (`DeliveryNoteBundlesCheckout`)**:  
The `carrierSignature` property is deprecated and will be removed.  
Instead, the `driverSignature` property must be used, as it is now required.

---

### `GET /delivery-notes/{deliveryNoteKey}`

**Response schema (`SingleDeliveryNote`)**:  
The `loadCarriers` property is deprecated and will be removed.  
Instead, the `loadCarriersConsignor` and `loadCarriersConsignee` properties must be used to get the load carriers for consignor and consignee respectively.

---

### `POST /organization-sites/{organizationSiteKey}/events`

**Request schema (`TriggerEvent`)**:  
The `checkinKey` property is deprecated and will be removed.  
Instead, the `checkinKeys` array property must be used, as it is now required.

**Response schema (`TriggerEventResponse`)**:  
The `eventKey` property is deprecated and will be removed.  
Instead, the `eventKeys` array property must be used, as it is now required.
