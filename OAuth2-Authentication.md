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
