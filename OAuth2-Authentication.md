# Client types

## Public Client

Ein Public Client ist eine Anwendung, die in einer unzuverlässigen Umgebung läuft (z. B. Browser/Native App) und kein Client-Secret sicher speichern kann. Deshalb authentifiziert er sich nicht mit einem Secret und muss PKCE verwenden.
Flow: Authorization Code mit PKCE (S256)

## Confidential Client

Ein Confidential Client ist eine serverseitige Anwendung, die ein Client-Secret sicher speichern und sich damit beim Authorization Server ausweisen kann. Der Token-Austausch erfolgt serverseitig.
Flow: Authorization Code mit Client-Authentifizierung (Secret)

## M2M (Machine-to-Machine)

Ein M2M-Client agiert ohne Benutzerkontext und authentifiziert ausschließlich die Anwendung selbst (Service-Account). Er erhält Tokens direkt auf Basis der Client-Anmeldedaten.
Flow: Client Credentials

# Authentication
Dieses Dokument beschreibt die wichtigsten Aspekte zur Authentifizierung als Service (maschine to maschine).

Alle wichtigen Authentication Endpunkte für die API-Test Umgebung können folgendem Dokument entnommen werden:
https://auth.dls.apitest.cloud4log.dev/realms/dls/.well-known/openid-configuration

Die Authentication funktioniert mit einer im Basic-Frontend erzeugten ClientId und ClientSecret. 
Dieser Client kann im Basic-Frontend im Admin-Panel unter Api-Clients als Unternehmensadmin angelegt werden.

This document describes the most important aspects of authentication as a service (machine to machine).

All important authentication endpoints for the API test environment can be found in the following document:
https://auth.dls.apitest.cloud4log.dev/realms/dls/.well-known/openid-configuration

Authentication works with a ClientId and ClientSecret generated in the Basic frontend. 
This client can be created as a company admin in the Basic frontend in the admin panel under Api clients.

## Example

```ts
import * as oidc from 'openid-client';

async function main() {
  const server = new URL('https://auth.dls.apitest.cloud4log.dev/realms/dls');
  const clientId = 'YOUR-CLIENT-ID';
  const clientSecret = 'YOUR-SECRET';

  const config = await oidc.discovery(server, clientId, clientSecret);

  const tokens = await oidc.clientCredentialsGrant(config);

  console.log('access_token:', tokens.access_token);

  const rsUrl = new URL('https://dlsbackend.apitest.cloud4log.dev/api/v2/me');
  const rsResp = await oidc.fetchProtectedResource(
    config,
    tokens.access_token!,
    rsUrl,
    'GET',
  );
  console.log('API /me status:', rsResp.status);
  console.log('API /me body:', await rsResp.text());
}

main().catch((e) => {
  console.error(e);
});
```
