---
name: Authenticate to ION and run a paginated GraphQL query
description: Exchange an ION API key for an OAuth access token and run a cursor-paginated GraphQL query against the ION Factory OS API.
api: https://api.buildwithion.com/graphql
method: generated
source: https://manual.firstresonance.io/api/access-tokens
operations:
- client_credentials token exchange (Keycloak api-keys realm)
- GraphQL query with Relay pagination (first/after, edges, pageInfo)
---

# Authenticate to ION and run a paginated query

Use this to make your first authenticated call to the ION Factory OS GraphQL API.

## 1. Get an API key
An ION admin creates an **API key** (a `clientId` / `clientSecret` pair) for your Organization in the ION UI or via the API. Keys are environment-specific: a production key will not work in sandbox. Prefer a **service account** so the key survives user deactivation.

## 2. Exchange the key for an access token
POST client credentials to the Keycloak `api-keys` realm token endpoint for your environment
(production: `auth.buildwithion.com`; staging/sandbox: `staging-auth.buildwithion.com`):

```
curl -X POST \
  --data-urlencode "grant_type=client_credentials" \
  -d "client_id=CLIENT_ID" \
  -d "client_secret=CLIENT_SECRET" \
  https://auth.buildwithion.com/realms/api-keys/protocol/openid-connect/token
```

## 3. Call the GraphQL API
Send the returned `access_token` on the `Authorization` header to the GraphQL endpoint
(`https://api.buildwithion.com/graphql`, staging `https://staging-api.buildwithion.com/graphql`).

## 4. Always paginate
ION uses Relay cursor pagination — pass `first` and `after`, and read back `edges` and
`pageInfo { endCursor hasNextPage totalCount count }`. Loop on `endCursor` while
`hasNextPage` is true. Supplying pagination params avoids network timeouts on large sets.

## Notes
- The API key inherits (freezes) the creating user's permissions at generation time.
- Errors come back in the GraphQL top-level `errors` array (not RFC 9457 problem+json).
- Discover the exact schema fields in the in-app **Interactive API Explorer**.
