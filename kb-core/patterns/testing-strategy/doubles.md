# Testing: Doubles and Isolation

**Category:** Testing · **Applies to:** All

## Doubles

| Type | Is                 | Use                |
| ---- | ------------------ | ------------------ |
| Stub | canned returns     | feed inputs        |
| Mock | asserts calls      | verify interaction |
| Fake | working light impl | in-mem DB          |
| Spy  | records calls      | check after        |

Default fakes over mocks; mock only at owned seams. Don't mock what you don't own (wrap it).

## Integration isolation

- Real DB via containers (testcontainers); reset per test (tx rollback / truncate).
- One reason to fail; no shared mutable state; parallel-safe.
- MSW/respx/httptest for outbound HTTP.

## Factories

Builders/factories for fixtures; sane defaults, override per test. No giant shared blobs.

## Related

`pyramid.md` · `kb-<lang>/testing/`
