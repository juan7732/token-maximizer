# Error Handling: Propagation

**Category:** Error Handling · **Applies to:** All

Default path: external → repository → service → handler. Lower layers enrich; the top decides.

```
external      raw failure (driver, syscall, HTTP)
repository    wrap: "load user 42" + cause
service       wrap: "renew subscription" + cause
handler       translate: status/exit, log internal, return user-facing
```

## Rules

- **Wrap, don't replace:** keep the cause, add operation name.
- **Translate once:** map to status/code/message at the boundary only.
- **Classify, don't lose:** carry taxonomy so boundary picks response + retry.
- **Log at the decision point:** one line with full chain, not per layer.

## Cancellation

Failure (broke) ≠ cancellation (caller gave up). Keep distinct so cancels aren't alerted.

## Related

`philosophy.md` · `taxonomy.md` · `kb-<lang>/idioms/error-handling.md`. [CAPTURE] real usage.
