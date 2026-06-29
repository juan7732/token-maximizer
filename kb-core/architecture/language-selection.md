# Language Selection

**Category:** Architecture · **Applies to:** All

Heuristic for which language a component wants. Default to team fluency; switch when a trait dominates.

| Reach for      | When                                                | Why                                               |
| -------------- | --------------------------------------------------- | ------------------------------------------------- |
| **Go**         | services, APIs, network/concurrency, CLIs, infra    | fast, easy concurrency, static bins, quick builds |
| **Rust**       | latency/memory critical, systems, data plane, no GC | control + safety, top perf                        |
| **TypeScript** | web/UI, BFF, light APIs, full-stack share           | one lang front+back, huge ecosystem               |
| **Python**     | ML/data, glue, scripts, prototypes                  | libraries, speed to write; hot paths leave it     |

## Tiebreakers

- Throughput/p99 + low memory → Rust. Good-enough + ship fast → Go.
- Shared types with frontend → TS. Models/data → Python; serve heavy load elsewhere.
- Polyglot: split by service boundary; talk via API/queue, not shared runtime.

## Defaults

API/service Go · perf core Rust · web TS · ML/glue Python. Override only on a clear trait. [CAPTURE] what worked.

## Related

`system-design-checklist.md` · kb-<lang> idioms
