# Service Patterns

**Category:** Architecture

**Monolith vs services:** start monolith; split when teams/scaling/deploy cadence force it. Distributed = network, partial failure, eventual consistency tax.

**Communication:**
- Sync: REST/gRPC for request-response. Timeouts + retries + circuit breakers mandatory.
- Async: queue/event bus for decoupling, fan-out, buffering. At-least-once + idempotency.

**Decompose** by bounded context, not by layer. Own your data; no shared DB.

**Resilience:** timeouts, retries (jittered), circuit breaker, bulkhead, backpressure, idempotency keys.

**Observability:** logs (structured), metrics (RED/USE), traces (propagate ids). Alert on SLO burn.

**See also:** scaling.md, data-architecture.md, kb-ops/runbooks/incident-response.md.
