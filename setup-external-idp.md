# Einbindung externer Identity Provider (IDP)

Dieser Abschnitt beschreibt den Prozess zur Integration externer Identity Provider (IDPs) in Cloud4Log.
Für die Einrichtung wird die Unterstützung durch einen Entwickler des externen IDPs benötigt.

## Unterstützte IDP-Protokolle:

- Keycloak OpenID Connect
- OAuth 2.0
- OpenID Connect 1.0
- SAML 2.0

## Keycloak OpenID Connect

Für die Anbindung an einen Keycloak-Server wird ein Confidential Client benötigt, der über die erforderlichen Berechtigungen verfügt.
Von diesem Client werden folgende Informationen benötigt:

- Client-Credentials (Client ID und Client Secret)
- Discovery Endpoint-URL des Keycloak-Realms (.well-known/openid-configuration)

Diese Daten dienen Cloud4Log zur Authentifizierung und automatischen Konfiguration der OIDC-Verbindung.

## OAuth 2.0

Für eine Integration über OAuth 2.0 werden die Standard-OAuth-Parameter benötigt:

- Discovery Endpoint
- Client ID und Client Secret
- ID-Claim, Username-Claim, Email-Claim

Cloud4Log nutzt diese Endpunkte, um den Authorization-Code-Flow durchzuführen und Access Tokens zu beziehen.

## OpenID Connect 1.0

Für eine Integration über OpenID Connect 1.0 werden die Standard-OAuth-Parameter benötigt:

- Discovery Endpoint
- Client ID und Client Secret

Cloud4Log nutzt diese Endpunkte, um den Authorization-Code-Flow durchzuführen und Access Tokens zu beziehen.

## SAML 2.0

Für eine SAML-basierte Integration werden die folgenden Informationen benötigt:

- Identity Provider (IdP) Metadata-XML oder die SSO URL
- Entity ID des IdP
- Optional: Attribute Mapping für Benutzerfelder
