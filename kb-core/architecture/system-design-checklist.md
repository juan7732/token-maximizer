# System Design Checklist

**Category:** Architecture · **Applies to:** All

A flow for design interviews and real designs. Talk tradeoffs, not buzzwords.

## Flow

1. **Scope:** functional + non-functional. Pin scale, read/write ratio, consistency, latency, SLA. State assumptions.
2. **Estimate:** QPS, storage, bandwidth. Reads vs writes. Peak = avg × 2–10.
3. **API:** core endpoints, request/response, sync vs async.
4. **Data model:** entities, access patterns first, then storage choice.
5. **High level:** client → LB → service → cache → DB; queues for async.
6. **Scale:** find bottlenecks, then cache/shard/replicate/queue.
7. **Tradeoffs:** call out CAP, consistency, cost, complexity. Name what you'd cut.

## Estimation cheats

- 1 day ≈ 86.4k s; 1M/day ≈ 12 QPS. ×3 peak.
- Storage = rows × size × retention × replication.
- 1 server ≈ 1–10k QPS; cache read ~ms, disk ~10ms, cross-region ~100ms.

## Defaults

| Need         | Reach for          |
| ------------ | ------------------ |
| Cache        | Redis              |
| Relational   | Postgres           |
| Doc/flexible | Mongo/Dynamo       |
| Queue/stream | Kafka / SQS        |
| Blob         | S3                 |
| Search       | Elastic/OpenSearch |
| Wide-column  | Cassandra/Scylla   |

## Levers

Stateless services + LB · cache hot reads · shard writes · read replicas · async via queue · CDN at edge · idempotency keys · backpressure + rate limit.

## Related

`scaling.md` · `language-selection.md`
