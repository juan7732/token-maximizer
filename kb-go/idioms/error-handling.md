# Go Error Handling

**Language:** Go · **Category:** Error Handling

Strategy: kb-core/patterns/error-handling. Here: Go mechanics.

**What:** errors are values, returned last, checked at each call. Wrap with `%w` to chain. `panic` for unrecoverable bugs only.

**Why:** visible failure beats invisible. `if err != nil` makes every fallible call a decision; the unused value is enforced.

```go
var ErrNotFound = errors.New("not found")        // sentinel: expected, comparable

type ValidationError struct{ Field, Msg string } // custom: callers need detail
func (e *ValidationError) Error() string { return e.Field + ": " + e.Msg }

func renew(id string) error {
    if err := store.Load(id); err != nil {
        return fmt.Errorf("renew %s: %w", id, err) // wrap + context
    }
    return nil
}

errors.Is(err, ErrNotFound)        // 404
var ve *ValidationError; errors.As(err, &ve) // 400, use ve.Field
```

**Gotchas:** `%v` not `%w` to hide a sentinel; wrap once per layer; `panic` only for bugs, recover at goroutine edges; typed-nil in interface is non-nil, return literal `nil`.

**See also:** kb-core error-handling; kb-rust/typescript/python idioms.
