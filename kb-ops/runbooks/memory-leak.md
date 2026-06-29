# Runbook: Memory Leak

**Category:** Ops · Runbook

1. Confirm: RSS climbs without plateau across deploys.
2. Snapshot heap (pprof Go, heap snapshot Node, tracemalloc Py, valgrind/heaptrack Rust).
3. Diff two snapshots; find growing allocation site.
4. Common causes: unbounded cache/map, goroutine/task leak, listeners not removed, retained closures.
5. Fix: bound caches (LRU), cancel tasks, remove handlers. Verify flat RSS.

**See also:** per-lang performance/profiling-guide.md.
