# Go Project Layout

**Language:** Go · **Category:** Idioms

- Flat until it hurts. One `main.go` is fine.
- `cmd/<app>/` entrypoints; `internal/` private; `pkg/` only if shared externally.
- Package by domain, not type (`user/`, not `models/`).
- Avoid circular imports; `internal/` enforces boundaries.
- One module per repo; `go.mod` at root.

```
cmd/api/main.go
internal/user/{service,store}.go
internal/http/router.go
go.mod
```

**Gotchas:** don't over-nest early; `pkg/` is noise for apps; keep main thin.

**See also:** idioms.md.
