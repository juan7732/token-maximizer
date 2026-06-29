# Rust Validation

**Language:** Rust · **Category:** Data Modeling

Strategy: kb-core/patterns/data-modeling. Here: Rust mechanics.

**What:** `serde` parse + `validator`; newtype for validated types so invalid states won't compile.

```rust
#[derive(Deserialize, Validate)]
struct CreateUser {
    #[validate(email)] email: String,
    #[validate(range(min = 0, max = 130))] age: u8,
}
let u: CreateUser = serde_json::from_str(body)?; u.validate()?;
```

**Gotchas:** newtype (`Email(String)`) for parse-once-trust-after; `#[serde(deny_unknown_fields)]` to reject extras; u8 already bounds age.

**See also:** kb-core data-modeling; kb-go/typescript/python.
