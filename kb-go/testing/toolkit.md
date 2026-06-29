# Go Testing

**Language:** Go · **Category:** Testing

Strategy: kb-core/patterns/testing-strategy. Here: Go toolkit.

**What:** stdlib `testing`, table-driven, `-race`, `testify` assert, `testcontainers-go` for integration.

```go
func TestAdd(t *testing.T) {
    cases := []struct{ a, b, want int }{{1,2,3}, {0,0,0}}
    for _, c := range cases {
        if got := Add(c.a, c.b); got != c.want {
            t.Errorf("Add(%d,%d)=%d want %d", c.a, c.b, got, c.want)
        }
    }
}
```

**Toolkit:** `go test -race`, `t.Parallel()`, `b.N` benchmarks, fuzz, golden files. **Gotchas:** loop-var capture, `t.Cleanup` over defer.

**See also:** kb-core testing-strategy.
