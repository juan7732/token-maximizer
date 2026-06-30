# TypeScript Type System

**Language:** TypeScript · **Category:** Idioms

Make illegal states unrepresentable; let inference work.

- **Unions + discriminant:** model state as `{kind:'a'} | {kind:'b'}`; switch narrows.
- **Generics:** constrain with `extends`; infer where possible.
- **Utility types:** `Partial/Pick/Omit/Record`; mapped + conditional for transforms.
- **Template literals:** typed string shapes.
- **Branded types:** `string & {__brand}` for validated values.
- **`unknown` not `any`:** narrow before use.

```ts
type Result<T> = { ok: true; value: T } | { ok: false; error: string };
type ID = string & { readonly __id: unique symbol };
```

**Gotchas:** `as` lies-prefer narrowing; types vanish at runtime (validate); avoid `enum`, use unions; `strict` always on.

**See also:** validation.md, concurrency.md, error-handling.md.
