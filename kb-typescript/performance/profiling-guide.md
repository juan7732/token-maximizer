# TypeScript Performance

**Language:** TypeScript · **Category:** Performance

Bundle + runtime both matter.

- **Bundle:** tree-shake, code-split, dynamic `import()`, analyze size.
- **Runtime:** avoid blocking loop; workers for CPU; stream big data.
- **Avoid:** giant sync JSON parse; megabyte deps; barrel-file import bloat.
- **Node:** stream files, pool connections, cache hot.

**Gotchas:** measure with profiler not vibes; `JSON.parse` blocks; GC pauses on big retained graphs; ship ESM for shaking.

**See also:** type-system.md, concurrency.md.
