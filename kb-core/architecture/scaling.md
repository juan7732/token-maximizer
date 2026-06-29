# Scaling

**Category:** Architecture · **Applies to:** All

Add a lever only when a bottleneck demands it. Each buys throughput, costs complexity.

## Caching

- Patterns: cache-aside (default), read/write-through, write-behind.
- Invalidate by TTL + event. Stampede: lock, jitter, early refresh.
- Tiers: client → CDN → app → DB cache. Hottest, smallest at top.

## Replication

- Read replicas for read-heavy; writes to primary. Replica lag → read-after-write goes to primary.
- Sync = consistent + slow; async = fast + stale risk.

## Sharding

- Split write/storage load. Key: hash (even), range (scans, hotspots), geo.
- Costs: cross-shard joins/tx, rebalancing. Use consistent hashing.

## Load balancing

- L4 fast, L7 routes. Round-robin / least-conn / hash. Health checks. Sticky only if stateful.

## Async + queues

- Queue (SQS) decouples; stream (Kafka) replays + fans out. Smooths spikes.
- At-least-once → idempotent consumers. DLQ for poison. Watch ordering.

## Consistency (CAP/PACELC)

- Partition: pick C or A. Strong = correct, costly. Eventual = available, stale.
- Quorum R+W>N. Idempotency keys, outbox for exactly-once-ish.

## Order

Stateless+LB → cache → replicas → shard → async/queues. Measure first.

## Related

`system-design-checklist.md`
