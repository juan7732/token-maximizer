# TypeScript Testing

**Language:** TypeScript · **Category:** Testing

Strategy: kb-core/patterns/testing-strategy. Here: TS toolkit.

**What:** Vitest/Jest, Testing Library (UI), MSW (HTTP fakes), Playwright (e2e), `tsd` (type tests).

```ts
test.each([[1,2,3],[0,0,0]])("add(%i,%i)=%i", (a,b,w) => {
  expect(add(a,b)).toBe(w);
});
```

**Toolkit:** `vitest`, MSW handlers, fixtures via factories, snapshot sparingly. **Gotchas:** mock at module boundary; fake timers for async; clean up between tests.

**See also:** kb-core testing-strategy.
