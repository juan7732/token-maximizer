# TypeScript Validation

**Language:** TypeScript · **Category:** Data Modeling

Strategy: kb-core/patterns/data-modeling. Here: TS mechanics.

**What:** Zod = runtime + compile-time. Parse at boundary, infer the type from the schema.

```ts
const CreateUser = z.object({ email: z.string().email(), age: z.number().min(0).max(130) });
type CreateUser = z.infer<typeof CreateUser>;
const r = CreateUser.safeParse(body);
if (!r.success) return badRequest(r.error.issues);
```

**Gotchas:** TS types vanish at runtime—validate input; `infer` keeps one source of truth; `.strict()` rejects extras; brand types for validated.

**See also:** kb-core data-modeling; kb-go/rust/python.
