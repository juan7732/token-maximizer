# API Design: Auth

**Category:** API Design · **Applies to:** All

## AuthN (who)

| Method         | Use                                           |
| -------------- | --------------------------------------------- |
| Session cookie | browser, server state                         |
| JWT            | stateless, short-lived; refresh token rotated |
| API key        | service/dev access; scope + rotate            |
| OAuth2/OIDC    | delegated, third-party login                  |
| mTLS           | service-to-service trust                      |

JWT: short expiry, verify sig + exp + aud, revoke via short TTL + denylist.

## AuthZ (what)

- **RBAC:** roles → permissions. Simple, default.
- **ABAC:** attributes (owner, dept, time). Fine-grained.
- Check at boundary; deny by default; never trust client claims.

## Rules

HTTPS only · secrets in vault not code · rate-limit auth · validate all input · don't leak which of user/pass wrong.

## Related

`rest.md` · `rpc.md` · kb-core security (planned)
