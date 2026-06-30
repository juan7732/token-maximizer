# Data Architecture

**Category:** Architecture

**DB choice:** Postgres default. Relational + ACID first. Reach elsewhere for proven need: KV (Redis), wide-column (Cassandra) for write scale, search (ES), timeseries, graph.

**Caching:** cache-aside default; write-through if read-heavy. TTL + invalidation. Beware stampede (lock/jitter), staleness.

**Consistency:** strong where money/identity; eventual where tolerable. Outbox for reliable events. Idempotent consumers.

**CQRS/event sourcing:** only when read/write models diverge or audit/replay needed. Heavy; not default.

**Migrations:** backward-compatible, expand/contract: add nullable, backfill, switch, drop. Reversible.

**Pipelines:** batch vs stream by latency need. Schema registry, dead-letter queue.

**See also:** service-patterns.md, kb-core/patterns/data-modeling/.
