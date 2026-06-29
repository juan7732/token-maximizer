# Data Modeling: Schema Evolution

**Category:** Data Modeling · **Applies to:** All

Change schema without downtime. Compatible changes only.

**Safe:** add nullable/defaulted column, add table, add optional field, widen type.
**Breaking:** drop/rename column, narrow type, change meaning, required-without-default.

## Zero-downtime rename (expand/contract)

1. Add new column.
2. Dual-write old + new.
3. Backfill.
4. Read new.
5. Drop old.

## Rules

- Migrations forward-only, idempotent, reversible.
- Protobuf: add fields, never reuse tags, reserve removed.
- Decouple deploy from migrate; tolerate both versions during rollout.

## Related

`modeling.md` · `serialization.md`
