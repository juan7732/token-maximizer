# Python Performance

**Language:** Python · **Category:** Performance

Slow until profiled. `cProfile`, `py-spy`, `tracemalloc`.

- **Hot loop → C:** numpy, vectorize, or PyO3/Cython.
- **CPU → processes** (GIL); I/O → async/threads.
- **Avoid:** per-iter attr lookups; build lists then join; use sets for membership.
- **`__slots__`**, generators for big data.

**Gotchas:** premature micro-opt; GIL blocks CPU threads; pickling cost across processes; profile real load.

**See also:** modern.md, concurrency.md.
