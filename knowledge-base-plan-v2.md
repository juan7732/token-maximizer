# Architectural Knowledge Base: Phased Implementation Plan (v2)

> **Purpose:** Build a durable, composable knowledge base as a set of repos that integrate
> into any project workspace via ergo, providing ambient AI context.
> **Core Languages:** Go · Rust · TypeScript · Python
> **Architecture:** Multi-repo, composed via ergo workspaces. Not a monolith you visit — a
> context layer you inhabit.
> **Token Strategy:** Front-load expensive generation with Opus; execute and iterate with Sonnet.

---

## How This Plan Changed (v1 → v2)

The original plan assumed a single `knowledge-base/` repo with subdirectories. With ergo,
the knowledge base becomes a **workspace of composable repos** that you pull selectively
into project workspaces. This means:

- Language-specific content lives in its own repo — only loaded when relevant
- Cross-cutting patterns stay unified — always available
- Templates and snippets become portable skills repos
- Project workspaces inherit knowledge as ambient context, not manual reference
- The knowledge base grows organically from project work via `[CAPTURE]` extraction

---

## Tenet: Incubate flat, graduate to repos

Content starts life in **flat folders inside this repo** — one folder per future
repo (`kb-core/`, `kb-go/`, `kb-templates/`, etc.). While a domain is being
drafted, its markdown lives in its folder here. This keeps early work in one
place: one window, easy cross-linking, no premature repo sprawl.

When a folder is ready, **split it out into its own repo** and pull it back as
an ergo reference repo (`--group=reference --tags=kb,reference`). Not a git
submodule — ergo is the composition layer, so reference repos give the same
single-window context without the pinned-SHA and detached-head friction
submodules bring. The folder leaves this repo; the dedicated repo joins the
workspace.

Result: this repo is the incubator and index; mature knowledge lives in
focused repos composed via ergo. A folder graduates only when it can stand on
its own.

---

## How to Use This Document

Each phase contains **modules** with concrete **deliverables** — artifacts you generate
and commit to the appropriate repo. Every deliverable is tagged:

- `[GEN]` — Generate with LLM assistance (spend tokens on these now)
- `[BUILD]` — Hands-on implementation work (light LLM use)
- `[CAPTURE]` — Extract patterns from your actual project work into the knowledge base
- `[TEMPLATE]` — Reusable boilerplate you'll copy-paste forever

You don't need to complete phases sequentially. Pick the modules most relevant to your
current projects and work outward. The numbering reflects dependency order, not priority.

---

## Phase 0: Foundation — The Repo Architecture

### 0.1 Repo Topology

The knowledge base is composed of these repos, each serving a distinct role:

```
Knowledge Base Repos
├── kb-core                    # Cross-cutting patterns (error handling, concurrency,
│                              # API design, data modeling, architecture, security)
├── kb-go                      # Go idioms, snippets, ecosystem, testing patterns
├── kb-rust                    # Rust idioms, snippets, ecosystem, testing patterns
├── kb-typescript              # TypeScript idioms, snippets, ecosystem, testing patterns
├── kb-python                  # Python idioms, snippets, ecosystem, testing patterns
├── kb-templates               # ADR, pattern, snippet, runbook, comparison templates
├── kb-ops                     # Checklists, runbooks, operational knowledge
└── kb-comparisons             # Cross-language Rosetta Stone translations
```

**Why this split:**

| Repo             | Why separate                                 | When you'd pull it in           |
| ---------------- | -------------------------------------------- | ------------------------------- |
| `kb-core`        | Always relevant, language-agnostic           | Every workspace                 |
| `kb-go`          | Only useful when writing Go                  | Go project workspaces           |
| `kb-rust`        | Only useful when writing Rust                | Rust project workspaces         |
| `kb-typescript`  | Only useful when writing TypeScript          | TS project workspaces           |
| `kb-python`      | Only useful when writing Python              | Python/ML workspaces            |
| `kb-templates`   | Structural scaffolding, not reference        | When starting new docs/patterns |
| `kb-ops`         | Operational, not development                 | Incident response, deployments  |
| `kb-comparisons` | Reference when translating between languages | Multi-language workspaces       |

### 0.2 Individual Repo Structure

Each language repo follows the same internal layout:

```
kb-go/                          # (same structure for rust, typescript, python)
├── README.md                   # Index of contents, when to use this repo
├── idioms/                     # Language-specific idiomatic patterns
│   ├── error-handling.md
│   ├── concurrency.md
│   ├── project-layout.md
│   └── ...
├── snippets/                   # Copy-paste-ready code with context
│   ├── http-server.md
│   ├── database-repository.md
│   ├── cli-scaffold.md
│   └── ...
├── ecosystem/                  # Library/framework selection guides
│   ├── http-frameworks.md
│   ├── database-drivers.md
│   ├── testing-tools.md
│   └── ...
├── testing/                    # Testing patterns and strategies
│   ├── table-driven-tests.md
│   ├── integration-testing.md
│   ├── mocking-patterns.md
│   └── ...
└── performance/                # Performance patterns and profiling
    ├── profiling-guide.md
    ├── allocation-patterns.md
    └── ...
```

The core repo has a different structure reflecting its cross-cutting nature:

```
kb-core/
├── README.md
├── patterns/                   # Design patterns across languages
│   ├── error-handling/
│   │   ├── philosophy.md       # When to use which error strategy
│   │   ├── taxonomy.md         # Error classification framework
│   │   └── propagation.md      # How errors flow through layers
│   ├── concurrency/
│   │   ├── mental-models.md    # Threads vs async vs message passing
│   │   ├── patterns.md         # Fan-out, pipeline, worker pool, etc.
│   │   └── debugging.md        # Deadlock/race condition diagnosis
│   ├── api-design/
│   ├── data-modeling/
│   └── testing-strategy/
├── architecture/               # System-level architectural patterns
│   ├── service-patterns/
│   ├── data-architecture/
│   ├── infrastructure/
│   └── security/
└── decisions/                  # Cross-project ADRs and decision frameworks
    ├── decision-frameworks/    # Reusable decision matrices
    └── reference-adrs/         # Example ADRs for common decisions
```

### 0.3 Deliverables

- `[BUILD]` Create all repos on GitHub with README stubs
- `[GEN]` Generate README.md for each repo: purpose, structure, index of contents, when to pull into a workspace
- `[GEN]` Generate all template files in `kb-templates` (see §0.4)
- `[BUILD]` Create the knowledge-base ergo workspace TOML (see §0.5)
- `[BUILD]` Create ergo workspace TOMLs for your active projects that include relevant kb repos

### 0.4 Templates Repo (`kb-templates`)

```
kb-templates/
├── README.md
├── pattern.md                  # Template for documenting a design pattern
├── adr.md                      # Architecture Decision Record template
├── runbook.md                  # Operational runbook template
├── snippet.md                  # Code snippet with context template
├── comparison.md               # Cross-language comparison template
├── idiom.md                    # Language-specific idiom template
└── ecosystem-guide.md          # Library/framework comparison template
```

Each template includes rich frontmatter prompts that guide both human authors
and AI assistants in filling them out consistently.

`[GEN]` **Pattern template:**
```markdown
# [Pattern Name]

**Category:** Concurrency | Error Handling | Data Modeling | API Design | ...
**Applies to:** Go | Rust | TypeScript | Python | All

## Problem

What problem does this pattern solve? When does it arise?

## Solution

Describe the pattern conceptually, independent of any language.

## Implementation Notes

### [Language]

Key considerations, idioms, and gotchas for this language.

(Code goes in the corresponding kb-<language>/snippets/ file, not here.)

## Tradeoffs

- **When to use:** ...
- **When to avoid:** ...
- **Complexity cost:** ...
- **Performance characteristics:** ...

## Related Patterns

- [Links to related patterns in this repo]

## Real-World Usage

Where have you used this? What did you learn? (Fill in via [CAPTURE])
```

`[GEN]` **ADR template:**
```markdown
# ADR-NNNN: [Title]

**Date:** YYYY-MM-DD
**Status:** Proposed | Accepted | Superseded by ADR-XXXX | Deprecated
**Project:** [Which project/workspace this decision was made in]

## Context

What forces are at play? What problem are we solving? What constraints exist?

## Decision

What is the change we're proposing or have made?

## Consequences

### Positive
- ...

### Negative
- ...

### Neutral
- ...

## Alternatives Considered

| Alternative | Pros | Cons | Why Rejected |
| ----------- | ---- | ---- | ------------ |
| ...         | ...  | ...  | ...          |

## References

- Links to relevant documentation, benchmarks, or discussions
```

`[GEN]` **Snippet template:**
```markdown
# [Descriptive Name]

**Language:** Go | Rust | TypeScript | Python
**Category:** HTTP | Database | CLI | Concurrency | ...
**Last Verified:** YYYY-MM-DD
**Dependencies:** list any non-stdlib dependencies

## When to Use

Situation where you'd reach for this snippet.

## Code

\`\`\`[language]
// Complete, copy-pasteable snippet
\`\`\`

## Customization Points

- Line N: change X for [different behavior]
- Line M: replace Z if [different use case]

## Gotchas

- Things that will bite you if you're not careful
```

`[GEN]` **Remaining templates:** runbook, comparison, idiom, ecosystem-guide
(similar structure — frontmatter prompts, consistent sections, `[CAPTURE]` placeholders)

### 0.5 Ergo Workspace Configurations

**The knowledge base authoring workspace** — for when you're writing/editing
knowledge base content:

```toml
# ~/.ergo/workspaces/knowledge-base.toml

[workspace]
name = "knowledge-base"
description = "Authoring workspace for all knowledge base repos"

[[repos]]
name = "kb-core"
url = "https://github.com/juan/kb-core.git"
tags = ["kb", "patterns"]
group = "knowledge"

[[repos]]
name = "kb-go"
url = "https://github.com/juan/kb-go.git"
tags = ["kb", "go"]
group = "knowledge"

[[repos]]
name = "kb-rust"
url = "https://github.com/juan/kb-rust.git"
tags = ["kb", "rust"]
group = "knowledge"

[[repos]]
name = "kb-typescript"
url = "https://github.com/juan/kb-typescript.git"
tags = ["kb", "typescript"]
group = "knowledge"

[[repos]]
name = "kb-python"
url = "https://github.com/juan/kb-python.git"
tags = ["kb", "python"]
group = "knowledge"

[[repos]]
name = "kb-templates"
url = "https://github.com/juan/kb-templates.git"
tags = ["kb", "templates"]
group = "knowledge"

[[repos]]
name = "kb-ops"
url = "https://github.com/juan/kb-ops.git"
tags = ["kb", "ops"]
group = "knowledge"

[[repos]]
name = "kb-comparisons"
url = "https://github.com/juan/kb-comparisons.git"
tags = ["kb", "reference"]
group = "knowledge"

[[folders]]
name = "scratch"
purpose = "Drafting space for new patterns and content"
```

**Example project workspace** — how kb repos compose into a real project:

```toml
# ~/.ergo/workspaces/handwriting-recognition.toml

[workspace]
name = "handwriting-recognition"
description = "Handwriting recognition model for the task pad"

# Project repos
[[repos]]
name = "handwriting-recognition"
url = "https://github.com/juan/handwriting-recognition.git"
tags = ["ml", "python", "project"]
group = "project"

[[repos]]
name = "task-pad"
url = "https://github.com/juan/task-pad.git"
tags = ["hardware", "python", "project"]
group = "project"

# Knowledge context (pulled in for AI to reference)
[[repos]]
name = "kb-core"
url = "https://github.com/juan/kb-core.git"
tags = ["kb"]
group = "knowledge"

[[repos]]
name = "kb-python"
url = "https://github.com/juan/kb-python.git"
tags = ["kb"]
group = "knowledge"

# Working space
[[folders]]
name = "planning"
purpose = "Architecture decisions, experiment logs, and design exploration"

[[folders]]
name = "data"
purpose = "Training data samples, evaluation results, model outputs"
```

Notice: this workspace includes `kb-core` and `kb-python` but NOT `kb-go`,
`kb-rust`, or `kb-typescript`. Copilot sees only relevant knowledge.

---

## Phase 1: Cross-Cutting Patterns (`kb-core`)

*Highest-leverage content. Lives in `kb-core` because it applies across all languages.
Pull this repo into every workspace.*

### 1.1 Error Handling (`kb-core/patterns/error-handling/`)

**Deliverables:**

- `[GEN]` **philosophy.md** — The spectrum from exceptions to Result types to error values. Not syntax — *when* to use which strategy and *why*. Decision framework for choosing an error handling approach based on project type, team size, and reliability requirements.
- `[GEN]` **taxonomy.md** — Reusable error classification: transient vs. permanent, retryable vs. fatal, user-facing vs. internal, expected vs. unexpected. Includes a template for defining project-specific error taxonomies.
- `[GEN]` **propagation.md** — How errors flow through layers (handler → service → repository → external) with architectural diagrams. Patterns for error wrapping, context enrichment, and boundary translation.

**Per-language implementation** goes in the respective `kb-<lang>/idioms/error-handling.md`:
- `[GEN]` **Go:** sentinel errors, custom error types, `errors.Is`/`errors.As`, wrapping, panic conventions
- `[GEN]` **Rust:** `Result<T, E>`, `thiserror` vs. `anyhow`, `?` operator, custom error enums, `From` impls
- `[GEN]` **TypeScript:** discriminated unions, `neverthrow`, error boundaries, Zod validation errors
- `[GEN]` **Python:** exception hierarchies, `@contextmanager`, structured logging, `ExceptionGroup`

**Per-language snippets** go in `kb-<lang>/snippets/custom-error-type.md`:
- `[TEMPLATE]` Custom error type starter for each language — copy-paste and customize

### 1.2 Concurrency and Parallelism (`kb-core/patterns/concurrency/`)

**Deliverables:**

- `[GEN]` **mental-models.md** — Threads vs. green threads vs. async, shared state vs. message passing, structured concurrency, backpressure, cancellation. Visual mental models, not API docs.
- `[GEN]` **patterns.md** — Concurrency pattern catalog: fan-out/fan-in, pipeline, worker pool, pub/sub, rate limiter, circuit breaker. Each pattern described conceptually with tradeoffs.
- `[GEN]` **debugging.md** — Deadlock/race condition diagnostic guide: symptoms, tools per language, prevention patterns.

**Per-language implementation** in `kb-<lang>/idioms/concurrency.md`:
- `[GEN]` **Go:** goroutines, channels, `sync`, `errgroup`, context cancellation, goroutine leak prevention
- `[GEN]` **Rust:** `tokio`, `async`/`await`, `Arc<Mutex<T>>`, channels, `Send`/`Sync`, `rayon`
- `[GEN]` **TypeScript:** event loop, `Promise.all`/`allSettled`, workers, `AbortController`, async iterators
- `[GEN]` **Python:** `asyncio`, `ThreadPoolExecutor`, `multiprocessing`, the GIL, `trio`

**Per-language snippets** in `kb-<lang>/snippets/`:
- `[GEN]` Worker pool, rate limiter, graceful shutdown — idiomatic per language

### 1.3 API Design (`kb-core/patterns/api-design/`)

**Deliverables:**

- `[GEN]` **rest-checklist.md** — Naming, versioning, pagination, filtering, error responses, idempotency
- `[GEN]` **grpc-guide.md** — Protobuf schema design, streaming patterns, error codes, interceptors, selection criteria
- `[GEN]` **graphql-decision-framework.md** — When it's worth the complexity, schema design, N+1 prevention, DataLoader
- `[GEN]` **auth-patterns.md** — JWT, OAuth 2.0, API keys, mTLS, RBAC vs. ABAC with implementation notes per language
- `[TEMPLATE]` **OpenAPI 3.1 starter** in `kb-templates/openapi-starter.yaml`
- `[TEMPLATE]` **Protobuf service definition starter** in `kb-templates/proto-starter.proto`

### 1.4 Data Modeling and Validation (`kb-core/patterns/data-modeling/`)

**Deliverables:**

- `[GEN]` **decision-tree.md** — Relational vs. document vs. key-value vs. graph vs. time-series with concrete selection criteria
- `[GEN]` **schema-evolution.md** — Backwards-compatible changes, migration strategies, zero-downtime patterns
- `[GEN]` **serialization-comparison.md** — JSON, MessagePack, Protobuf, CBOR: performance and selection criteria

**Per-language validation guides** in `kb-<lang>/idioms/validation.md`:
- `[GEN]` **Go:** struct tags, `ozzo-validation`, custom validators
- `[GEN]` **Rust:** `serde` + `validator`, newtype pattern
- `[GEN]` **TypeScript:** Zod, io-ts, branded types, runtime vs. compile-time
- `[GEN]` **Python:** Pydantic v2, dataclasses + validators, `cattrs`

### 1.5 Testing Strategy (`kb-core/patterns/testing-strategy/`)

**Deliverables:**

- `[GEN]` **pyramid.md** — Testing pyramid tailored to your stack: unit, integration, e2e, contract, property-based, fuzzing
- `[GEN]` **test-architecture.md** — Table-driven tests, fixture management, test doubles taxonomy, integration isolation

**Per-language testing toolkits** in `kb-<lang>/testing/`:
- `[GEN]` **Go:** `testing`, `testify`, `gomock`, `testcontainers-go`, `-race`, benchmarks
- `[GEN]` **Rust:** `#[cfg(test)]`, `proptest`, `mockall`, `rstest`, `criterion`
- `[GEN]` **TypeScript:** Vitest, Testing Library, MSW, Playwright, `tsd`
- `[GEN]` **Python:** pytest, `hypothesis`, `factory_boy`, `respx`, `testcontainers-python`
- `[TEMPLATE]` Test file starters in `kb-<lang>/snippets/test-starter.md`

---

## Phase 2: Language Deep-Dives (`kb-go`, `kb-rust`, `kb-typescript`, `kb-python`)

*Go deep on the "grain" of each language. Each section below populates the corresponding
language repo. These repos get pulled into workspaces selectively.*

### 2.1 Go (`kb-go`)

**Idioms (`idioms/`):**
- `[GEN]` **go-idioms.md** — Accept interfaces/return structs, functional options, table-driven tests, embedding, iota
- `[GEN]` **concurrency.md** — Patterns beyond basics: semaphore channels, graceful shutdown, dynamic worker pools
- `[GEN]` **performance.md** — Escape analysis, stack vs. heap, `sync.Pool`, slice pre-allocation, pprof/trace
- `[GEN]` **project-layout.md** — When flat is fine, packages, `internal/`, avoiding circular deps
- `[GEN]` **generics.md** — Constraints, type sets, generic data structures, when generics vs. interfaces

**Ecosystem (`ecosystem/`):**
- `[GEN]` **http-frameworks.md** — net/http, chi, fiber, echo: selection criteria and starter patterns
- `[GEN]` **database-drivers.md** — sqlx, GORM, pgx: when to use each
- `[GEN]` **testing-tools.md** — testify, gomock, testcontainers: toolkit comparison

**Snippets (`snippets/`):**
- `[GEN]` HTTP server with middleware stack, graceful shutdown, health check
- `[GEN]` Repository pattern with sqlx
- `[GEN]` CLI scaffold with cobra
- `[GEN]` Worker pool with errgroup
- `[TEMPLATE]` Service skeleton: HTTP + logging + config + graceful shutdown

### 2.2 Rust (`kb-rust`)

**Idioms (`idioms/`):**
- `[GEN]` **ownership.md** — Moves, borrows, lifetimes through real scenarios
- `[GEN]` **traits.md** — Trait objects vs. generics, blanket impls, sealed traits, orphan rule
- `[GEN]` **error-design.md** — Library vs. application errors, `anyhow` vs. `thiserror`, conversion chains
- `[GEN]` **async.md** — Executor model, pinning, `Send`/`Sync` in practice, cancellation safety
- `[GEN]` **unsafe-ffi.md** — When unsafe is justified, soundness, FFI, `bindgen`
- `[GEN]` **performance.md** — Zero-cost abstractions, SIMD, `cargo flamegraph`, allocation-free patterns

**Ecosystem (`ecosystem/`):**
- `[GEN]` **http-frameworks.md** — axum vs. actix-web: selection criteria
- `[GEN]` **database-drivers.md** — sqlx, diesel, sea-orm: comparison
- `[GEN]` **crate-map.md** — Go-to crate for each common need with rationale

**Snippets (`snippets/`):**
- `[GEN]` axum server with tower middleware, tracing, graceful shutdown
- `[GEN]` Custom error type with `thiserror`
- `[GEN]` CLI scaffold with clap
- `[GEN]` Async worker with tokio channels
- `[TEMPLATE]` Service skeleton: axum + tracing + config + shutdown

### 2.3 TypeScript (`kb-typescript`)

**Idioms (`idioms/`):**
- `[GEN]` **type-system.md** — Conditional types, mapped types, template literals, `infer`, branded types
- `[GEN]` **patterns-at-scale.md** — Barrel files, DI without frameworks, module boundaries, monorepos
- `[GEN]` **runtime-safety.md** — Where types end and runtime validation begins, Zod integration
- `[GEN]` **node-operations.md** — Graceful shutdown, health checks, structured logging, config, secrets

**Ecosystem (`ecosystem/`):**
- `[GEN]` **http-frameworks.md** — Express, Fastify, Hono: comparison
- `[GEN]` **database-drivers.md** — Prisma, Drizzle, Knex: when to use each
- `[GEN]` **testing-tools.md** — Vitest, Testing Library, MSW, Playwright

**Snippets (`snippets/`):**
- `[GEN]` Fastify/Hono server with middleware, validation, error handling
- `[GEN]` Zod schema with type inference
- `[GEN]` React component patterns (if relevant to your projects)
- `[TEMPLATE]` Service skeleton: Fastify + pino + config + shutdown

### 2.4 Python (`kb-python`)

**Idioms (`idioms/`):**
- `[GEN]` **modern-python.md** — Dataclasses vs. Pydantic vs. attrs, pattern matching, protocols, `__slots__`
- `[GEN]` **packaging.md** — pyproject.toml, uv, poetry: decision framework and setup
- `[GEN]` **async.md** — asyncio, `httpx`, async context managers, when async is overkill
- `[GEN]` **systems-interop.md** — ctypes, cffi, PyO3, memory management, `py-spy`
- `[GEN]` **cli-tooling.md** — click vs. typer vs. argparse, rich output, progress bars

**Ecosystem (`ecosystem/`):**
- `[GEN]` **http-frameworks.md** — FastAPI, Starlette, Django: comparison
- `[GEN]` **database-drivers.md** — SQLAlchemy, Tortoise: when to use each
- `[GEN]` **ml-stack.md** — PyTorch, MLX, scikit-learn, data loading, experiment tracking

**Snippets (`snippets/`):**
- `[GEN]` FastAPI service with Pydantic models, dependency injection
- `[GEN]` MLX training loop with data loading and checkpointing
- `[GEN]` CLI with typer and rich output
- `[TEMPLATE]` ML project scaffold: training pipeline + evaluation + export

---

## Phase 3: System Architecture (`kb-core/architecture/`)

*System-level patterns that help you make architectural decisions without LLM consultation.*

### 3.1 Service Architecture (`architecture/service-patterns/`)

- `[GEN]` **decision-framework.md** — Monolith vs. modular monolith vs. microservices decision matrix
- `[GEN]` **communication-patterns.md** — Sync vs. async, saga, outbox, exactly-once delivery
- `[GEN]` **decomposition.md** — Bounded contexts, data ownership, cross-cutting concerns
- `[GEN]` **observability.md** — Structured logging, OpenTelemetry, Prometheus, SLO/SLI design
- `[TEMPLATE]` Service skeleton in `kb-go/snippets/` and `kb-rust/snippets/`

### 3.2 Data Architecture (`architecture/data-architecture/`)

- `[GEN]` **database-selection.md** — PostgreSQL, SQLite, Redis, DynamoDB, ClickHouse with workload recommendations
- `[GEN]` **caching-strategy.md** — Cache-aside, read-through, write-through, invalidation, stampede prevention
- `[GEN]` **event-sourcing-cqrs.md** — When worth the complexity, implementation patterns, projections
- `[GEN]` **data-pipelines.md** — Batch vs. stream, exactly-once, backfill, schema registry, dead letters
- `[GEN]` **migration-playbook.md** — Zero-downtime migrations, dual-write, feature flags, rollback

### 3.3 Infrastructure (`architecture/infrastructure/`)

- `[GEN]` **container-patterns.md** — Multi-stage builds per language, distroless, security scanning, layer caching
- `[GEN]` **ci-cd-reference.md** — GitHub Actions pipeline architectures for Go, Rust, TS, Python
- `[GEN]` **iac-comparison.md** — Terraform vs. Pulumi vs. CDK with starters
- `[GEN]` **local-dev.md** — Docker Compose, dev containers, Tilt/Skaffold, Nix

**Templates** in `kb-templates/`:
- `[TEMPLATE]` Dockerfile starters for each language (optimized multi-stage)
- `[TEMPLATE]` GitHub Actions workflows: lint, test, build, deploy per language
- `[TEMPLATE]` docker-compose starters for common development stacks
- `[TEMPLATE]` Justfile/Makefile starters per language

### 3.4 Security (`architecture/security/`)

- `[GEN]` **app-security-checklist.md** — OWASP Top 10 mapped to your stack with concrete mitigations
- `[GEN]` **auth-architecture.md** — Sessions, JWT, OAuth 2.0 + OIDC, passkeys — tradeoffs
- `[GEN]` **secrets-management.md** — Env vars, Vault, cloud KMS, SOPS, `age` — with local dev patterns
- `[GEN]` **supply-chain.md** — Dependency pinning, lockfile verification, SBOM, vulnerability scanning

---

## Phase 4: Operational Knowledge (`kb-ops`)

*Things you look up every time. Document once, reference forever. This repo gets pulled
into workspaces when you're debugging, deploying, or handling incidents.*

### 4.1 Runbooks (`kb-ops/runbooks/`)

- `[GEN]` **database-troubleshooting.md** — Slow queries, connection pools, lock contention, replication lag
- `[GEN]` **container-troubleshooting.md** — CrashLoopBackOff, OOMKilled, networking, DNS, resource limits
- `[GEN]` **memory-leak-diagnosis.md** — Per-language tools: Go pprof, Rust DHAT, Node heap snapshots, Python tracemalloc
- `[GEN]` **incident-response.md** — Severity classification, communication templates, postmortem structure
- `[CAPTURE]` Project-specific runbooks from your own deployments

### 4.2 Checklists (`kb-ops/checklists/`)

- `[GEN]` **new-project-bootstrap.md** — Per language: structure, linting, formatting, testing, CI, docs, deps, license
- `[GEN]` **code-review.md** — Error handling, edge cases, concurrency, security, performance, tests, docs
- `[GEN]` **pre-deployment.md** — Migrations, feature flags, rollback plan, monitoring, alerting, load testing
- `[GEN]` **dependency-update.md** — Changelog, breaking changes, advisories, test coverage, gradual rollout
- `[GEN]` **post-incident.md** — Evidence, timeline, root cause, action items, communication

---

## Phase 5: Cross-Language Comparisons (`kb-comparisons`)

*The Rosetta Stone repo. Pull this in when you need to translate a pattern you know
in one language into another. Especially useful in multi-language workspaces.*

### 5.1 Pattern Translations

- `[GEN]` **iterators-streams.md** — Go (for-range, channels), Rust (Iterator), TS (generators, async iterables), Python (generators, itertools)
- `[GEN]` **builder-pattern.md** — Functional options (Go), builder derive (Rust), fluent interfaces (TS), dataclass-based (Python)
- `[GEN]` **dependency-injection.md** — wire/fx (Go), manual/shaku (Rust), tsyringe/inversify (TS), dependency-injector (Python)
- `[GEN]` **state-machines.md** — Enums + match (Rust), iota + switch (Go), discriminated unions (TS), class-based (Python)
- `[GEN]` **cli-parsing.md** — cobra (Go), clap (Rust), commander/yargs (TS), click/typer (Python)

### 5.2 Ecosystem Maps

- `[GEN]` **http-frameworks.md** — Cross-language comparison with selection criteria
- `[GEN]` **orm-database.md** — Cross-language ORM/driver comparison
- `[GEN]` **serialization.md** — Cross-language serialization library comparison
- `[GEN]` **logging-observability.md** — slog (Go), tracing (Rust), pino (TS), structlog (Python)

---

## Phase 6: Local LLM Setup

*This content lives in `kb-core/local-llm/` since it's language-agnostic and
relevant to any workspace.*

### 6.1 Ollama on Apple Silicon (`kb-core/local-llm/`)

- `[BUILD]` Install and configure Ollama, verify Metal acceleration
- `[GEN]` **model-selection.md** — Benchmarked recommendations by RAM tier (16GB / 32GB / 64GB+)
- `[GEN]` **quantization-guide.md** — Q4_K_M vs. Q5_K_M vs. Q8_0 tradeoffs on M-series
- `[BUILD]` Modelfile library: custom system prompts for code review, refactoring, docs, debugging
- `[GEN]` **prompt-templates.md** — Prompt patterns optimized for smaller models

### 6.2 IDE and Workflow Integration

- `[BUILD]` Continue.dev setup pointing at local Ollama
- `[GEN]` **task-routing.md** — Which tasks to local vs. cloud, with concrete examples
- `[BUILD]` Shell aliases and scripts for piping code through local models

### 6.3 Advanced Local Setup

- `[BUILD]` llama.cpp direct setup for maximum inference control
- `[GEN]` **rag-setup.md** — Index your knowledge base repos for RAG with local models
- `[GEN]` **fine-tuning-guide.md** — LoRA/QLoRA on your code patterns using MLX

---

## Phase 7: Workflow Automation

*Reduce LLM dependency with deterministic tooling. This content is split between
`kb-core` (language-agnostic) and per-language repos.*

### 7.1 Code Generation Templates

Per-language repos get generator configs:
- `[BUILD]` **Go:** `go generate` templates for CRUD handlers, repository interfaces, mocks
- `[BUILD]` **Rust:** `cargo-generate` templates for common patterns
- `[BUILD]` **TypeScript:** Plop.js or Hygen generators for components, API routes, tests
- `[BUILD]` **Python:** Cookiecutter templates for project scaffolding

### 7.2 Editor Productivity (`kb-core/workflow/`)

- `[GEN]` **language-server-mastery.md** — gopls, rust-analyzer, tsserver, Pyright: features you're not using
- `[GEN]` **editor-snippets.md** — VS Code snippets per language covering patterns in this knowledge base
- `[GEN]` **linter-formatter-configs.md** — Opinionated configs for golangci-lint, clippy, ESLint+Prettier, Ruff
- `[BUILD]` Git hooks via `lefthook` or `pre-commit`: format, lint, type-check, test

### 7.3 Shell and CLI Automation (`kb-core/workflow/`)

- `[GEN]` **justfile-reference.md** — Portable task runner with common targets per language
- `[GEN]` **shell-patterns.md** — Safe bash, error handling, arg parsing, color output
- `[BUILD]` Personal CLI scripts: project scaffolding, dependency auditing, port management
- `[TEMPLATE]` Justfile starters per language in `kb-templates/`

---

## Execution Strategy

### Token Budget Allocation (Remaining Window)

| Priority | Category                         | % Budget | What You're Generating                             |
| -------- | -------------------------------- | -------- | -------------------------------------------------- |
| 1        | Phase 1 (`kb-core` patterns)     | 30%      | Highest reuse — pulled into every workspace        |
| 2        | Phase 2 (language repos)         | 25%      | Deep idioms eliminate most repeated queries        |
| 3        | Phase 3 (`kb-core` architecture) | 20%      | Architectural decisions are expensive LLM queries  |
| 4        | Phase 5 (`kb-comparisons`)       | 10%      | Rosetta stone — learn once, apply across languages |
| 5        | Phase 6 (local LLM setup)        | 10%      | Build fallback infrastructure                      |
| 6        | Phase 4, 7 (ops, automation)     | 5%       | Lower LLM leverage, more hands-on                  |

### Daily Workflow with ergo

1. **Start of day:** Open your project workspace with `ergo open`
2. **KB repos are already there** — Copilot/Sonnet has your patterns as ambient context
3. **Work on your project** — reference patterns naturally, AI sees them in context
4. **Extract patterns:** When you discover something reusable, write it up using a
   template from `kb-templates` and commit to the appropriate kb repo
5. **End of day:** `ergo run --group=knowledge -- git add -A && git commit -m "..."` to
   commit knowledge base updates across all kb repos at once

### Composing Workspaces for Your Projects

Here's how kb repos map to your 30 Before 30 projects:

| Project                      | Workspace Repos (project) | KB Repos to Include                      |
| ---------------------------- | ------------------------- | ---------------------------------------- |
| Handwriting recognition (#1) | handwriting-recognition   | kb-core, kb-python                       |
| ergo CLI                     | ergo                      | kb-core, kb-go                           |
| Video game (#5)              | bevy-game                 | kb-core, kb-rust                         |
| Personal tools (#2)          | library-app, finance-app  | kb-core, kb-go, kb-typescript            |
| iPhone app (#12)             | thirty-under-thirty       | kb-core, kb-typescript                   |
| Startup MVP                  | startup-cli, startup-web  | kb-core, kb-go, kb-typescript, kb-python |
| Kaggle (#20)                 | kaggle-competition        | kb-core, kb-python                       |
| E-ink display (#26)          | eink-display              | kb-core, kb-python or kb-rust            |

### Incremental Value Checkpoints

After each phase, you should notice specific categories of LLM queries disappearing:

- **After Phase 0:** ergo is running, kb repos compose into project workspaces
- **After Phase 1:** You stop asking "how do I handle errors in X" and "what concurrency pattern for Y" — the answer is in your workspace context
- **After Phase 2:** You stop asking "what's idiomatic in [language]" — Copilot infers it from your idioms repo
- **After Phase 3:** You stop asking "should I use X or Y architecture" — decision frameworks are ambient
- **After Phase 5:** You stop asking "how do I do [thing from Go] in Rust" — the comparison is right there
- **After Phase 6:** Routine coding tasks use zero cloud tokens
- **After Phase 7:** Boilerplate generation is fully automated

### The Sonnet Optimization Payoff

The entire architecture is designed to make Sonnet perform at Opus level:

1. **Context is pre-loaded** — kb repos in the workspace mean Sonnet doesn't need to infer your patterns
2. **Templates constrain output** — Sonnet follows templates excellently, reducing the need for creative architectural reasoning
3. **Decisions are pre-made** — decision frameworks mean you've already done the hard thinking
4. **Idioms are documented** — Sonnet doesn't need to figure out what's idiomatic, it reads your guide
5. **Snippets are copy-paste** — the most common code patterns don't need generation at all

The workspace IS the optimization. A well-structured ergo workspace with kb repos makes the model matter less.