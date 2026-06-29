# Concurrency: Debugging

**Category:** Concurrency · **Applies to:** All

## Symptoms

- **Deadlock:** stuck, no CPU. Cyclic lock wait or full unread channel.
- **Race:** flaky, data corruption. Unsynced shared access.
- **Leak:** memory/goroutine grows. No exit or cancel.
- **Livelock/starvation:** busy, no progress. Retry storm or unfair locks.

## Tools

Go `-race`, pprof goroutines · Rust loom, tsan, tokio-console · TS leak/heap profiler · Python faulthandler, py-spy.

## Prevent

- One lock order; minimal critical sections.
- Always cancel/close; defer unlock.
- Bound queues; timeout blocking ops.
- Test under load + race detector.

## Related

`mental-models.md` · `patterns.md`
