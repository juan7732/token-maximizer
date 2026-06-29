# Go Generics

**Language:** Go · **Category:** Idioms

- Use for containers/algorithms, not to replace interfaces.
- Constraints: `comparable`, `any`, custom `~int` sets.
- Don't generify when an interface method works.

```go
func Map[T,U any](s []T, f func(T)U) []U {
  r := make([]U, len(s)); for i,v := range s { r[i]=f(v) }; return r
}
type Number interface { ~int | ~float64 }
func Sum[T Number](s []T) T { var t T; for _,v:=range s{t+=v}; return t }
```

**Gotchas:** no methods on type params; inference limits; interface often simpler.

**See also:** idioms.md.
