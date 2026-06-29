# Go Validation

**Language:** Go · **Category:** Data Modeling

Strategy: kb-core/patterns/data-modeling. Here: Go mechanics.

**What:** struct tags + validator, or `ozzo-validation` for programmatic. Validate at boundary, convert to domain types.

```go
type CreateUser struct {
    Email string `json:"email" validate:"required,email"`
    Age   int    `json:"age"   validate:"gte=0,lte=130"`
}
if err := validate.Struct(in); err != nil { return badReq(err) }
```

**Gotchas:** zero value vs absent (use pointers/`omitempty`); validate before mapping to domain; tags are strings, no compile check.

**See also:** kb-core data-modeling; kb-rust/typescript/python.
