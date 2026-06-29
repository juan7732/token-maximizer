# Rust Error Handling

**Language:** Rust · **Category:** Error Handling

Strategy: kb-core/patterns/error-handling. Here: Rust mechanics.

**What:** failure is `Result<T, E>`. `?` propagates and converts. `thiserror` for libs (typed enums), `anyhow` for apps (one type). `panic` for bugs.

**Why:** compiler forces handling, so unhandled failure is a build error.

```rust
#[derive(thiserror::Error, Debug)]
enum RenewError {
    #[error("user {0} not found")] NotFound(String),
    #[error(transparent)] Db(#[from] sqlx::Error),  // From conversion
}
fn renew(id: &str) -> Result<(), RenewError> { let u = load(id)?; save(u)?; Ok(()) }

fn run() -> anyhow::Result<()> { renew("42").context("renewing")?; Ok(()) }
```

**Gotchas:** `thiserror` libs / `anyhow` apps; `unwrap`/`expect` only when impossible or tests; isolate cross-boundary panics with `catch_unwind`; add `.context` at boundaries.

**See also:** kb-core error-handling; kb-go/typescript/python idioms.
