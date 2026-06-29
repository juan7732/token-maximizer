# Data Modeling: Decision Tree

**Category:** Data Modeling · **Applies to:** All

Model access patterns first, store second.

| Store                   | When                                              |
| ----------------------- | ------------------------------------------------- |
| Relational (PG)         | relations, transactions, ad-hoc queries. Default. |
| Document (Mongo)        | flexible/nested, few joins, per-doc atomicity     |
| Key-value (Redis)       | cache, sessions, simple fast lookup               |
| Wide-column (Cassandra) | massive write, known queries, time-series         |
| Graph (Neo4j)           | deep relationships, traversal                     |
| Time-series             | metrics, append-heavy, time windows               |

## Rules

- Start relational unless a trait clearly dominates.
- Normalize for writes, denormalize for reads. Pick per query load.
- Index access paths; index has write + storage cost.
- Polyglot per service; one store per bounded context.

## Related

`schema-evolution.md` · `serialization.md` · `kb-<lang>/idioms/validation.md`
