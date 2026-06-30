# Code Generation

**Category:** Ops · Automation

- Generate boilerplate, not logic. Scaffolding, types, clients.
- **Per language:** Go `go generate` + stringer/sqlc; Rust `build.rs` + macros; TS codegen from Zod/OpenAPI; Python cookiecutter/copier.
- **From schema:** OpenAPI → clients (openapi-generator); protobuf → stubs (buf); SQL → types (sqlc, kysely-codegen).
- Commit generated code or generate in CI, pick one and document it.
- LLM scaffolding: feed a template + spec, review output. Treat as draft.

**Gotchas:** never hand-edit generated files; pin generator versions; keep templates in `kb-templates/`.

**See also:** kb-templates/, justfile-starter.
