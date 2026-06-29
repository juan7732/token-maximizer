# Concurrency: Mental Models

**Category:** Concurrency · **Applies to:** All

Pick the model the problem wants. Mechanics: `kb-<lang>/idioms/concurrency.md`.

## Concurrency vs parallelism

Concurrency = structure many tasks; parallelism = run many at once. Concurrent design can run on one core.

## Execution

- **Threads:** OS-scheduled, preempt, ~MB stacks. CPU-bound.
- **Green/goroutines:** runtime-scheduled, cheap. Many tasks.
- **Async/event loop:** single thread, non-blocking I/O. I/O-bound.

## State

- **Shared + locks:** fast, deadlock/race risk.
- **Message passing:** channels/queues; own data, pass ownership.
- Rule: share nothing where you can; isolate then communicate.

## Discipline

- **Structured concurrency:** children bound to parent scope; no orphans.
- **Cancellation:** propagate via context/token; check at await points.
- **Backpressure:** bound queues; block or drop, never unbounded.

## CPU vs I/O

CPU-bound → parallel, ≈ core count. I/O-bound → async, high count fine.

## Related

`patterns.md` · `debugging.md`
