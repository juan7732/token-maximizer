# Concurrency: Patterns

**Category:** Concurrency · **Applies to:** All

Catalog. Code in `kb-<lang>/snippets/`.

| Pattern         | Use                     | Note                                           |
| --------------- | ----------------------- | ---------------------------------------------- |
| Fan-out/fan-in  | parallelize then merge  | bound workers; collect on one channel          |
| Pipeline        | staged transform        | each stage own goroutine; close to signal done |
| Worker pool     | cap concurrency         | fixed workers off shared queue                 |
| Pub/sub         | one→many events         | per-sub buffers; slow sub blocks or drops      |
| Rate limiter    | cap throughput          | token bucket; smooth bursts                    |
| Circuit breaker | stop calling broken dep | closed→open→half-open; fail fast               |
| Semaphore       | limit resource          | buffered channel / counting sem                |
| Future/promise  | await later             | compose, set timeout                           |

## Defaults

- Cap with worker pool, not unbounded spawn.
- Bound queues; backpressure over OOM.
- Timeout + cancel every blocking call.
- Breaker + retry/backoff on remote deps.

## Related

`mental-models.md` · `debugging.md`
