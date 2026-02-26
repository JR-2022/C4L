# OAuth 2.1 Authentication

This document describes how to authenticate with the Cloud4Log API using OAuth 2.1 and Keycloak.

With **Cloud4Log Basic v2.0.0**, **OAuth 2.1** was introduced as the new standard for authentication. **Keycloak** serves as the identity provider.

> **Note:** The legacy authentication endpoints (`POST /authentication/login`, `POST /authentication/loginTwoFARequest`, `POST /authentication/logout`) are deprecated and will be removed in a future release (v3.0.0). Please migrate to OAuth 2.1 as soon as possible.

---

## Environments

| Environment    | OpenID Configuration (Discovery Endpoint)                                              | API Base URL                     |
| -------------- | -------------------------------------------------------------------------------------- | -------------------------------- |
| **API Test**   | https://auth.nonprod.cloud4log.dev/realms/dls-apitest/.well-known/openid-configuration | https://api.cloud4log-apitest.de |
| **Test**       | https://auth.nonprod.cloud4log.dev/realms/dls-test/.well-known/openid-configuration    | https://api.cloud4log-test.de    |
| **UAT**        | https://auth.nonprod.cloud4log.dev/realms/dls-uat/.well-known/openid-configuration     | https://api.cloud4log-uat.de     |
| **Production** | https://auth.cloud4log.com/realms/dls/.well-known/openid-configuration                 | https://api.cloud4log.com        |

All important authentication endpoints (token endpoint, authorization endpoint, JWKS URI, etc.) can be obtained from the OpenID Configuration (Discovery Endpoint) for the respective environment.

---

## API Client Setup

Authentication works with a ClientId and ClientSecret (or PKCE) generated via the Cloud4Log Basic Frontend.

To create an API client:

1. Log in to the **Basic Frontend**.
2. Navigate to the **Admin Panel** as an **Org Admin**.
3. Under **"API Clients"**, create a new client.
4. Select the **organization sites** for which the client should be valid.

The API supports three client types, each suited for a different use case:

---

## Client Types

### Public Client

A Public Client is an application that runs in an untrusted environment (e.g., browser, native app) and cannot securely store a client secret. Therefore, it does not authenticate with a secret and must use PKCE.

- **Flow:** Authorization Code with PKCE (S256)
- **Use case:** Third-party frontend applications
- **Required parameters at creation:**
  - `clientId` — a unique name for the client
  - `redirectUris` — at least one redirect URI
  - `webOrigins` — allowed web origins (CORS)
  - `organizationSiteKeysAndRoles` — the sites and roles to assign

### Confidential Client (User Login)

A Confidential Client is a server-side application that can securely store a client secret and authenticate with the authorization server. The token exchange happens server-side.

- **Flow:** Authorization Code with Client Authentication (Secret) and PKCE (S256)
- **Use case:** Server-side applications with user context
- **Required parameters at creation:**
  - `clientId` — a unique name for the client
  - `m2m: false`
  - `redirectUris` — at least one redirect URI
  - `webOrigins` — allowed web origins
  - `organizationSiteKeysAndRoles` — the sites and roles to assign

### M2M (Machine-to-Machine)

An M2M client acts without a user context and authenticates only the application itself (service account). It receives tokens directly based on the client credentials.

- **Flow:** Client Credentials
- **Use case:** Machine-to-machine communication, backend integrations, automated processes
- **Required parameters at creation:**
  - `clientId` — a unique name for the client
  - `m2m: true`
  - `organizationSiteKeysAndRoles` — the sites and roles to assign

---

## Authentication Flows

### M2M (Client Credentials) — Recommended for API Integrations

This is the simplest flow for server-to-server integrations where no user context is needed.

**Step 1:** Obtain an access token from the token endpoint using your Client ID and Client Secret.

**Step 2:** Use the access token in the `Authorization` header for all API requests.

```
Authorization: Bearer <ACCESS_TOKEN>
```

### Authorization Code with PKCE — For User-Based Access

This flow is used for Public Clients and Confidential User Login Clients where a user needs to log in.

**Step 1:** Redirect the user to the Keycloak authorization endpoint with the required parameters (including PKCE `code_challenge` with method `S256`).

**Step 2:** After the user logs in, Keycloak redirects back to your `redirectUri` with an authorization code.

**Step 3:** Exchange the authorization code for tokens at the token endpoint (include the PKCE `code_verifier`).

**Step 4:** Use the access token in the `Authorization` header for all API requests.

```
Authorization: Bearer <ACCESS_TOKEN>
```

---

## API Authorization

All API requests must include the access token in the `Authorization` header:

```
Authorization: Bearer <ACCESS_TOKEN>
```

The API supports two versions:

- **API v2:** `https://<API_BASE_URL>/api/v2/...`
- **API v3:** `https://<API_BASE_URL>/api/v3/...`

> **Note:** Bearer tokens are signed with RS256 and are valid for a limited time (typically 5 minutes). Use token refresh mechanisms as needed.

### Legacy API Key (Deprecated)

The API still accepts the legacy `dlssessionid` header for backward compatibility. This method will be removed in a future release. Please migrate to Bearer Token authentication.

---

## Code Examples

### Example 1: M2M Authentication with `openid-client` (TypeScript)

```ts
import * as oidc from "openid-client";

async function main() {
  // Use the discovery endpoint for your target environment:
  // API Test: https://auth.nonprod.cloud4log.dev/realms/dls-apitest
  // Test:     https://auth.nonprod.cloud4log.dev/realms/dls-test
  // UAT:      https://auth.nonprod.cloud4log.dev/realms/dls-uat
  // Prod:     https://auth.cloud4log.com/realms/dls
  const server = new URL("https://auth.nonprod.cloud4log.dev/realms/dls");
  const clientId = "<YOUR-CLIENT-ID>";
  const clientSecret = "<YOUR-CLIENT-SECRET>";

  const config = await oidc.discovery(server, clientId, clientSecret);

  const tokens = await oidc.clientCredentialsGrant(config);

  console.log("access_token:", tokens.access_token);

  // Use the API base URL for your target environment:
  // API Test: https://api.cloud4log-apitest.de
  // Test:     https://api.cloud4log-test.de
  // UAT:      https://api.cloud4log-uat.de
  // Prod:     https://api.cloud4log.com
  const rsUrl = new URL("https://api.cloud4log-apitest.de/api/v2/me");
  const rsResp = await oidc.fetchProtectedResource(
    config,
    tokens.access_token!,
    rsUrl,
    "GET",
  );
  console.log("API /me status:", rsResp.status);
  console.log("API /me body:", await rsResp.text());
}

main().catch((e) => {
  console.error(e);
});
```

### Example 2: M2M Authentication with `fetch` (JavaScript / Node.js)

```js
const tokenEndpoint =
  "https://auth.nonprod.cloud4log.dev/realms/dls/protocol/openid-connect/token";
const clientId = "<YOUR-CLIENT-ID>";
const clientSecret = "<YOUR-CLIENT-SECRET>";

const params = new URLSearchParams({
  grant_type: "client_credentials",
  client_id: clientId,
  client_secret: clientSecret,
});

fetch(tokenEndpoint, {
  method: "POST",
  headers: { "Content-Type": "application/x-www-form-urlencoded" },
  body: params.toString(),
})
  .then(async (res) => {
    if (!res.ok) throw new Error(await res.text());
    return res.json();
  })
  .then((data) => {
    console.log("access_token:", data.access_token);

    fetch("https://api.cloud4log-apitest.de/api/v2/me", {
      method: "GET",
      headers: {
        Authorization: "Bearer " + data.access_token,
      },
    })
      .then(async (res) => {
        if (!res.ok) throw new Error(await res.text());
        console.log(await res.json());
      })
      .catch(console.error);
  })
  .catch(console.error);
```

### Example 3: M2M Authentication with `curl` (Bash)

```bash
# Step 1: Obtain access token
TOKEN=$(curl -s -X POST \
  'https://auth.nonprod.cloud4log.dev/realms/dls/protocol/openid-connect/token' \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -d 'grant_type=client_credentials' \
  -d 'client_id=<YOUR-CLIENT-ID>' \
  -d 'client_secret=<YOUR-CLIENT-SECRET>' | jq -r '.access_token')

echo "Access Token: $TOKEN"

# Step 2: Call the API
curl -s -X GET \
  'https://api.cloud4log-apitest.de/api/v2/me' \
  -H "Authorization: Bearer $TOKEN" | jq .
```

---

## Further Resources

- **[API Documentation v2](https://api.cloud4log.com/api-docs/)** — Stable version for existing integrations.
- **[API Documentation v3](https://api.cloud4log.com/api-docs-v3/)** — New version with pagination support and additional features.
- **[Combined API Documentation (v2 + v3)](https://api.cloud4log.com/api-docs-combined/)** — Both versions side by side.
- **[Integration of external Identity Provider (IDP)](https://github.com/JR-2022/C4L/blob/main/setup-external-idp.md)** — How to connect your own Keycloak or SAML identity provider.
- **[FAQ](https://github.com/JR-2022/C4L/blob/main/FAQ.md)** — Frequently asked questions.
