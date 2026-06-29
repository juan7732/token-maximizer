# Go Idioms

**Language:** Go · **Category:** Idioms

Beyond error/concurrency (own files). The Go grain.

- **Accept interfaces, return structs.** Callers depend on behavior, get concrete types.
- **Functional options:** `New(opts ...Option)` for optional config, not giant structs.
- **Composition over inheritance:** embed for reuse; no class tree.
- **Small interfaces:** 1-3 methods (`io.Reader`). Define at use site, not impl.
- **iota** for enums; pair with `String()`.
- **Zero value useful:** make structs work unconfigured (`sync.Mutex`).
- **defer** for cleanup; runs LIFO; watch loops.

```go
type Option func(*Server)
func WithPort(p int) Option { return func(s *Server){ s.port=p } }
func New(opts ...Option) *Server { s:=&Server{port:8080}; for _,o:=range opts{o(s)}; return s }
```

**Gotchas:** init() order/hidden; circular imports → `internal/`; pointer vs value receiver consistency; nil slice ok, nil map panics on write.

**See also:** concurrency.md, error-handling.md; layout.md.
