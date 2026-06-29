# TypeScript Concurrency

**Language:** TypeScript · **Category:** Concurrency

Strategy: kb-core/patterns/concurrency. Here: TS/Node mechanics.

**What:** single-threaded event loop; concurrency via async I/O. `Promise.all` parallel, worker_threads for CPU, `AbortController` to cancel.

```ts
const ac = new AbortController();
const results = await Promise.allSettled(
  urls.map(u => fetch(u, { signal: ac.signal }))
);
// pool: cap via p-limit
const limit = pLimit(8);
await Promise.all(tasks.map(t => limit(() => run(t))));
```

**Gotchas:** CPU work blocks loop → workers; `Promise.all` fails fast, use `allSettled` to keep partials; unhandled rejection crashes; await in loop serializes.

**See also:** kb-core concurrency; kb-go/rust/python.
