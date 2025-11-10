# Einbindung externer Identity Provider (IDP)

Dieser Abschnitt beschreibt den Prozess zur Integration externer Identity Provider (IDPs) in Cloud4Log.

## Unterstützte IDP-Protokolle:

- Keycloak OpenID Connect
- OAuth 2.0
- OpenID Connect 1.0
- SAML 2.0

## Keycloak OpenID Connect

Für die Anbindung an einen Keycloak-Server wird ein Confidential Client benötigt, der über die erforderlichen Berechtigungen verfügt.
Von diesem Client werden folgende Informationen benötigt:

Client-Credentials (Client ID und Client Secret)

Discovery Endpoint-URL des Keycloak-Realms (.well-known/openid-configuration)

Diese Daten dienen Cloud4Log zur Authentifizierung und automatischen Konfiguration der OIDC-Verbindung.
