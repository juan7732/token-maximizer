# API Design: RPC and GraphQL

**Category:** API Design · **Applies to:** All

## Choose

| Style   | When                                                                |
| ------- | ------------------------------------------------------------------- |
| REST    | public, CRUD, cache-friendly, broad clients                         |
| gRPC    | internal service-to-service, low latency, streaming, typed contract |
| GraphQL | many clients, varied shapes, aggregate many sources                 |

## gRPC

- Proto = contract; codegen clients. Bin + HTTP/2, fast.
- Streaming: server, client, bidi. Status codes not HTTP. Interceptors = middleware.
- Evolve: add fields, never reuse tags, reserve removed.

## GraphQL

- One endpoint, client picks fields. Solves over/under-fetch.
- Cost: N+1 (use DataLoader), caching hard, depth/complexity limits needed.
- Worth it with many client shapes; else REST.

## Related

`rest.md` · `auth.md` · protobuf starter in kb-templates
