# TypeScript Error Handling

**Language:** TypeScript · **Category:** Error Handling

Strategy: kb-core/patterns/error-handling. Here: TS mechanics.

**What:** throw for bugs; model expected failures as discriminated unions (or `neverthrow`). Validate input at boundaries with Zod.

**Why:** `throw` is untyped; unions put outcomes in the type, forcing the caller to handle both arms.

```ts
type Result<T, E> = { ok: true; value: T } | { ok: false; error: E };
function renew(id: string): Result<User, "not_found" | "expired"> {
  const u = users.get(id);
  return u ? { ok: true, value: u } : { ok: false, error: "not_found" };
}
const r = renew("42"); if (!r.ok) return reply(r.error); // forced branch

const parsed = z.object({ email: z.string().email() }).safeParse(body);
if (!parsed.success) return badRequest(parsed.error.issues);
```

**Gotchas:** `catch (e)` is `unknown`, narrow first; handle async rejections or Node crashes; one union per domain, map to status at boundary; branded types block unvalidated values.

**See also:** kb-core error-handling; kb-go/rust/python idioms.
