# TypeScript at Scale

**Language:** TypeScript · **Category:** Idioms

- `strict` always; ban `any`, prefer `unknown` at edges.
- Project references + path aliases for monorepos.
- Discriminated unions over enums for state.
- Zod at boundaries, infer types from schemas.
- Branded types for ids; readonly for invariants.
- No `tsc` runtime; build with tsup/esbuild.

```ts
type Cents = number & { __c: true };
type Result<T> = {ok:true;v:T}|{ok:false;e:string};
type User = z.infer<typeof UserSchema>;
```

**Gotchas:** structural typing leaks; enum vs union; barrel files kill build perf.

**See also:** type-system.md, ecosystem/map.md.
