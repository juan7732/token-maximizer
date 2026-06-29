# Testing: Pyramid

**Category:** Testing · **Applies to:** All

Many fast unit, fewer integration, few e2e. Invert and CI gets slow and flaky.

| Level       | Scope                      | Count        | Speed  |
| ----------- | -------------------------- | ------------ | ------ |
| Unit        | one fn/module, no I/O      | most         | ms     |
| Integration | modules + real dep (DB)    | some         | s      |
| E2E         | full stack, user path      | few          | slow   |
| Contract    | API both sides agree       | per boundary | fast   |
| Property    | invariants on random input | targeted     | varies |

## Rules

- Test behavior, not internals. Refactor shouldn't break tests.
- Deterministic: no sleeps, fixed seeds/clock.
- One reason to fail per test. Name = what + expected.
- Cover edges/errors, not just happy path. Coverage is a floor, not a goal.

## Related

`doubles.md` · `kb-<lang>/testing/`
