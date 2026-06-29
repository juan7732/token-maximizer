# Go Performance

**Language:** Go · **Category:** Performance

Measure first: `pprof`, `go test -bench`, `-benchmem`, trace.

- **Escape analysis:** stack > heap. Avoid pointer escapes; `go build -gcflags=-m`.
- **Preallocate:** `make([]T, 0, n)`; reuse with `sync.Pool`.
- **Strings:** `strings.Builder`, not `+` in loops.
- **Slices:** beware retaining big backing array; copy to free.
- **Avoid alloc:** value receivers, no premature interface boxing.

```go
var b strings.Builder
b.Grow(n)
for _, s := range parts { b.WriteString(s) }
```

**Gotchas:** profile don't guess; concurrency != speed; GC tuned by `GOGC`; benchmark stable env.

**See also:** idioms.md, concurrency.md.
