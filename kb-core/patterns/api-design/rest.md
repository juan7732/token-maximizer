# API Design: REST

**Category:** API Design · **Applies to:** All

**Resources:** nouns, plural (`/users/42/orders`). Verbs = HTTP methods. Nest one level max.

**Methods:** GET read, POST create, PUT replace, PATCH partial, DELETE. GET/PUT/DELETE idempotent.

**Status:** 200 ok, 201 created, 204 no body, 400 bad input, 401/403 authn/authz, 404, 409 conflict, 422 validation, 429 rate, 5xx server.

**Pagination:** cursor over offset for scale. `?limit=&cursor=`, return next cursor.

**Filtering:** `?status=active&sort=-created`. Document defaults.

**Versioning:** `/v1` in path (simple) or header (clean). Never break v1.

**Errors:** consistent shape `{code, message, details}`. Stable codes, human message.

**Idempotency:** `Idempotency-Key` on POST so retries don't double-charge.

**Other:** ETags + conditional, rate-limit headers, gzip, HTTPS only.

## Related

`rpc.md` · `auth.md` · OpenAPI starter in kb-templates
