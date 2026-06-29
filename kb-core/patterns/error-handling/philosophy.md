# Error Handling: Philosophy

**Category:** Error Handling · **Applies to:** All

When to use which strategy and why. Mechanics live in `kb-<lang>/idioms/error-handling.md`.

## Spectrum (implicit → explicit)

- **Exceptions** (Python, JS throw): invisible second channel. Concise, but signature hides what fails. Easy to ignore.
- **Error values** (Go): failure is a return value. Verbose, visible, hard to ignore.
- **Result types** (Rust, TS unions): failure in the type system. Compiler forces handling. Most safety, most ceremony.

## Choosing

- **Reliability:** higher cost of failure → push toward Result. Scripts can throw and crash.
- **Boundary vs core:** boundaries (net/disk/parse) fail routinely → model as data. Core bugs → panic/throw.
- **Audience:** libraries make failure explicit; apps collapse to one top handler.
- **Tooling:** Result pays off only with compiler enforcement; else exceptions + lint + review.

## Expected vs exceptional

Expected (bad input, not found) = contract, return as value. Exceptional (OOM, broken invariant, bug) = fail loud. Conflating them gives noise or silent corruption.

## Default

- Expected failures as values; exceptions/panics for bugs only.
- Visible at boundaries; one handler at top.
- Loud in dev, graceful in prod.

## Related

`taxonomy.md` · `propagation.md` · `kb-<lang>/idioms/error-handling.md`. [CAPTURE] real usage.
