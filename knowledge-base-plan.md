# Architectural Knowledge Base: Phased Implementation Plan

> **Purpose:** Build a durable, language-aware knowledge base that reduces LLM dependency over time.
> **Core Languages:** Go · Rust · TypeScript · Python
> **Philosophy:** Every artifact generated should be reusable, composable, and self-documenting.
> **Token Strategy:** Front-load expensive generation now; harvest value indefinitely.

---

## How to Use This Document

This plan is designed to be executed incrementally. Each phase contains **modules**, and each module contains **deliverables** — concrete artifacts you'll generate (mostly with LLM assistance while tokens are cheap) and store in your knowledge base.

You don't need to complete phases sequentially. Pick the modules most relevant to your current projects and work outward. The numbering reflects dependency order, not priority.

Every deliverable is tagged:

- `[GEN]` — Generate with LLM assistance (use your token budget on these)
- `[BUILD]` — Hands-on implementation work (light LLM use)
- `[CAPTURE]` — Document decisions/patterns from existing projects
- `[TEMPLATE]` — Reusable boilerplate you'll copy-paste forever

---

## Phase 0: Foundation — The Knowledge Base Itself

*Before generating content, build the container. This phase is lightweight and mostly manual.*

### 0.1 Choose Your Medium

Recommended stack for an M-series Mac developer:

| Option | Strengths | When to Choose |
|---|---|---|
| **Obsidian** (local markdown vault) | Fast, local-first, graph view, plugin ecosystem | You want zero cloud dependency and love linked thinking |
| **mdBook** (Rust-based) | Generates searchable static sites from markdown | You want a browsable reference site you can host or keep local |
| **Git repo + markdown** | Version-controlled, greppable, portable | You want maximum simplicity and already live in the terminal |
| **Notion** | Rich media, databases, sharing | You collaborate with others frequently |

**Recommendation:** A Git repo with well-structured markdown, optionally rendered with mdBook. It's greppable, versionable, diffable, and will outlive any SaaS product.

### 0.2 Repository Structure

```
knowledge-base/
├── README.md                    # This plan (or a trimmed version)
├── _templates/                  # Reusable document templates
│   ├── pattern.md               # Template for documenting a pattern
│   ├── adr.md                   # Architecture Decision Record template
│   ├── runbook.md               # Operational runbook template
│   ├── snippet.md               # Code snippet with context template
│   └── comparison.md            # Language/tool comparison template
├── 01-patterns/                 # Design patterns, idioms, architectures
│   ├── concurrency/
│   ├── error-handling/
│   ├── data-modeling/
│   ├── api-design/
│   └── ...
├── 02-idioms/                   # Language-specific idioms and conventions
│   ├── go/
│   ├── rust/
│   ├── typescript/
│   └── python/
├── 03-architectures/            # System-level architectural patterns
│   ├── service-patterns/
│   ├── data-pipelines/
│   ├── event-driven/
│   └── ...
├── 04-toolchain/                # Build systems, CI/CD, dev environment
│   ├── build-systems/
│   ├── testing-strategies/
│   ├── ci-cd/
│   └── local-dev/
├── 05-decisions/                # ADRs from your actual projects
│   └── YYYY-MM-DD-title.md
├── 06-runbooks/                 # Operational procedures
├── 07-snippets/                 # Battle-tested code snippets
│   ├── go/
│   ├── rust/
│   ├── typescript/
│   └── python/
├── 08-checklists/               # Pre-flight checklists for common tasks
├── 09-comparisons/              # Cross-language and cross-tool comparisons
└── 10-local-models/             # Notes on local LLM setup and usage
```

### 0.3 Deliverables

- `[BUILD]` Initialize the repo with the directory structure above
- `[GEN]` Generate all five `_templates/` files with rich frontmatter and section prompts
- `[BUILD]` Set up mdBook or your chosen renderer (optional)
- `[BUILD]` Create a `Makefile` or `justfile` with commands: `new-pattern`, `new-adr`, `new-snippet`, `build`, `search`

---

## Phase 1: Cross-Cutting Patterns

*These are the highest-leverage artifacts. They apply across all four languages and every project you touch. Generate these first.*

### 1.1 Error Handling Strategies

One of the areas where LLMs get asked the most repetitive questions. Document it once, reference forever.

**Deliverables:**

- `[GEN]` **Error handling philosophy guide** — covering the spectrum from exceptions (Python) to Result types (Rust) to error values (Go) to union types (TypeScript). Not just syntax — *when* to use which strategy and *why*.
- `[GEN]` **Error taxonomy template** — a reusable classification: transient vs. permanent, retryable vs. fatal, user-facing vs. internal, expected vs. unexpected.
- `[GEN]` **Per-language error handling cheat sheets:**
  - Go: sentinel errors, custom error types, `errors.Is`/`errors.As`, wrapping conventions, when to panic
  - Rust: `Result<T, E>`, `thiserror` vs. `anyhow`, the `?` operator, custom error enums, `From` implementations
  - TypeScript: discriminated unions for errors, `neverthrow`, error boundaries in React, Zod validation errors
  - Python: exception hierarchies, `@contextmanager` for cleanup, structured logging of exceptions, `ExceptionGroup`
- `[GEN]` **Error propagation patterns** — how errors flow through layers (handler → service → repository → external) in each language
- `[TEMPLATE]` **Custom error type starter** for each language — copy-paste and customize

### 1.2 Concurrency and Parallelism

Another area where people lean on LLMs heavily because the mental models are genuinely hard.

**Deliverables:**

- `[GEN]` **Concurrency mental model guide** — covering: threads vs. green threads vs. async, shared state vs. message passing, structured concurrency, backpressure, cancellation
- `[GEN]` **Per-language concurrency reference:**
  - Go: goroutines, channels, `sync` package, `errgroup`, context cancellation, common footguns (goroutine leaks, race conditions)
  - Rust: `tokio` runtime, `async`/`await`, `Arc<Mutex<T>>`, channels (`mpsc`, `broadcast`), `Send`/`Sync` traits, `rayon` for data parallelism
  - TypeScript: event loop deep-dive, `Promise.all` vs. `Promise.allSettled`, worker threads, `AbortController`, async iterators
  - Python: `asyncio` event loop, `ThreadPoolExecutor`, `multiprocessing`, the GIL and when it matters, `trio` as an alternative
- `[GEN]` **Concurrency pattern catalog:** fan-out/fan-in, pipeline, worker pool, pub/sub, rate limiter, circuit breaker — with idiomatic implementations in all four languages
- `[GEN]` **Deadlock/race condition diagnostic guide** — symptoms, debugging tools, prevention patterns

### 1.3 API Design

**Deliverables:**

- `[GEN]` **REST API design checklist** — naming, versioning, pagination, filtering, error responses, HATEOAS (when it matters), idempotency
- `[GEN]` **gRPC design guide** — protobuf schema design, streaming patterns, error codes, interceptors, when to choose gRPC over REST
- `[GEN]` **GraphQL decision framework** — when it's worth the complexity, schema design principles, N+1 prevention, DataLoader pattern
- `[GEN]` **API authentication/authorization patterns** — JWT, OAuth 2.0 flows, API keys, mTLS, RBAC vs. ABAC — with implementation notes per language
- `[TEMPLATE]` **OpenAPI 3.1 starter template** with best-practice structure
- `[TEMPLATE]` **Protobuf service definition starter** with common patterns

### 1.4 Data Modeling and Validation

**Deliverables:**

- `[GEN]` **Data modeling decision tree** — relational vs. document vs. key-value vs. graph vs. time-series, with concrete selection criteria
- `[GEN]` **Schema evolution guide** — backwards-compatible changes, migration strategies, zero-downtime schema changes
- `[GEN]` **Validation pattern guide per language:**
  - Go: struct tags, custom validators, `ozzo-validation`
  - Rust: `serde` + `validator`, newtype pattern for validated types
  - TypeScript: Zod, io-ts, branded types, runtime vs. compile-time validation
  - Python: Pydantic v2, dataclasses + validators, `cattrs`
- `[GEN]` **Serialization/deserialization comparison** — JSON, MessagePack, Protobuf, CBOR — performance characteristics and when to use each

### 1.5 Testing Strategies

**Deliverables:**

- `[GEN]` **Testing pyramid guide** tailored to your stack — unit, integration, e2e, contract, property-based, fuzzing
- `[GEN]` **Test architecture patterns:**
  - Table-driven tests (Go, Rust)
  - Fixture management and factories
  - Test doubles taxonomy: mocks vs. stubs vs. fakes vs. spies — when each is appropriate
  - Integration test isolation: containers, in-memory databases, test transactions
- `[GEN]` **Per-language testing toolkit reference:**
  - Go: `testing` stdlib, `testify`, `gomock`, `testcontainers-go`, `go test -race`, benchmarks
  - Rust: `#[cfg(test)]`, `proptest`, `mockall`, `rstest`, `criterion` for benchmarks
  - TypeScript: Vitest, Testing Library, MSW for API mocking, Playwright, type-level testing with `tsd`
  - Python: pytest, `hypothesis` for property-based testing, `factory_boy`, `respx`/`responses`, `testcontainers-python`
- `[TEMPLATE]` **Test file starters** for each language with common patterns pre-wired

---

## Phase 2: Language-Specific Deep Dives

*These go beyond syntax into the idiomatic "grain" of each language. The goal is to internalize the mental models so deeply you stop needing to ask.*

### 2.1 Go Mastery Module

**Deliverables:**

- `[GEN]` **Go idioms compendium** — accept interfaces/return structs, functional options, table-driven tests, embedding for composition, init() pitfalls, iota patterns
- `[GEN]` **Go concurrency cookbook** — patterns beyond the basics: semaphore via buffered channel, graceful shutdown orchestration, worker pool with dynamic sizing, pipeline with cancellation
- `[GEN]` **Go performance guide** — escape analysis, stack vs. heap, sync.Pool, string building, slice pre-allocation, pprof and trace workflows
- `[GEN]` **Go project layout reference** — when flat is fine, when packages matter, internal/ convention, avoiding circular dependencies
- `[GEN]` **Go generics patterns** — constraints, type sets, generic data structures, when generics help vs. when interfaces are simpler
- `[CAPTURE]` **Lessons learned** from your Go projects — document gotchas you've hit

### 2.2 Rust Mastery Module

**Deliverables:**

- `[GEN]` **Ownership mental model guide** — moves, borrows, lifetimes explained through real scenarios, not textbook examples
- `[GEN]` **Rust trait design patterns** — trait objects vs. generics, blanket implementations, sealed traits, extension traits, the orphan rule and workarounds
- `[GEN]` **Rust error design playbook** — library errors vs. application errors, when to use `anyhow` vs. `thiserror`, error conversion chains
- `[GEN]` **Rust async deep-dive** — executor model, pinning, `Send`/`Sync` bounds in practice, structured concurrency with `tokio`, cancellation safety
- `[GEN]` **Rust FFI and unsafe guide** — when unsafe is justified, soundness obligations, FFI calling conventions, `bindgen` workflows
- `[GEN]` **Rust performance guide** — zero-cost abstractions in practice, SIMD, `cargo flamegraph`, allocation-free patterns, `SmallVec`/`ArrayVec`
- `[GEN]` **Cargo ecosystem map** — the go-to crate for each common need, with brief rationale

### 2.3 TypeScript Mastery Module

**Deliverables:**

- `[GEN]` **TypeScript type system deep-dive** — conditional types, mapped types, template literal types, infer keyword, type narrowing strategies, branded/opaque types
- `[GEN]` **TypeScript patterns for scale** — barrel files (pros/cons), dependency injection without frameworks, module boundaries, monorepo patterns with workspaces
- `[GEN]` **Runtime vs. compile-time safety guide** — where TS types end and runtime validation begins, Zod/io-ts integration patterns, type-safe API clients
- `[GEN]` **Node.js operational patterns** — graceful shutdown, health checks, structured logging, configuration management, secrets handling
- `[GEN]` **Frontend architecture patterns** (if relevant) — state management comparison, component composition, server components, data fetching patterns
- `[GEN]` **TypeScript performance guide** — bundle analysis, tree-shaking, dynamic imports, worker threads, stream processing

### 2.4 Python Mastery Module

**Deliverables:**

- `[GEN]` **Modern Python patterns** — dataclasses vs. Pydantic vs. attrs, structural pattern matching, walrus operator, `__slots__`, protocols (structural subtyping)
- `[GEN]` **Python packaging and dependency management** — pyproject.toml, uv, poetry, conda — decision framework and setup guides
- `[GEN]` **Python async patterns** — asyncio, `aiohttp`, `httpx`, async context managers, async generators, when async is overkill
- `[GEN]` **Python for systems programmers** — ctypes, cffi, PyO3 (Rust bindings), memory management, profiling with `cProfile`/`py-spy`
- `[GEN]` **Python CLI tooling guide** — `click` vs. `typer` vs. `argparse`, rich terminal output, progress bars, configuration file patterns

---

## Phase 3: System Architecture Patterns

*Move from code-level to system-level. These artifacts help you make architectural decisions without needing to ask an LLM to think through tradeoffs.*

### 3.1 Service Architecture

**Deliverables:**

- `[GEN]` **Architecture decision framework** — monolith vs. modular monolith vs. microservices, with a concrete decision matrix based on team size, deployment frequency, and domain complexity
- `[GEN]` **Service communication patterns** — synchronous (REST, gRPC) vs. asynchronous (queues, event streams), saga pattern, outbox pattern, exactly-once delivery strategies
- `[GEN]` **Service decomposition guide** — bounded context identification, data ownership, shared-nothing architecture, managing cross-cutting concerns
- `[GEN]` **Observability stack reference** — structured logging, distributed tracing (OpenTelemetry), metrics (Prometheus), alerting philosophy, SLO/SLI design
- `[TEMPLATE]` **Service skeleton** in Go and Rust — HTTP server, health check, graceful shutdown, structured logging, configuration, middleware stack — all pre-wired

### 3.2 Data Architecture

**Deliverables:**

- `[GEN]` **Database selection guide** — PostgreSQL, SQLite, Redis, DynamoDB, ClickHouse, ScyllaDB, etc. — with workload-specific recommendations and concrete benchmarks
- `[GEN]` **Caching strategy guide** — cache-aside, read-through, write-through, write-behind, cache invalidation strategies, TTL design, cache stampede prevention
- `[GEN]` **Event sourcing and CQRS guide** — when it's worth the complexity, implementation patterns, projection design, snapshotting, replay strategies
- `[GEN]` **Data pipeline patterns** — batch vs. stream processing, exactly-once semantics, backfill strategies, schema registry, dead letter queues
- `[GEN]` **Migration strategy playbook** — zero-downtime migrations, dual-write patterns, feature flags for data changes, rollback strategies

### 3.3 Infrastructure and Deployment

**Deliverables:**

- `[GEN]` **Container patterns guide** — multi-stage builds (optimized for each language), distroless images, security scanning, layer caching strategies
- `[GEN]` **CI/CD pipeline reference architectures** — GitHub Actions, with pipeline templates for Go, Rust, TypeScript, and Python projects
- `[GEN]` **Infrastructure as Code comparison** — Terraform vs. Pulumi vs. CDK, with starter templates
- `[GEN]` **Local development environment guide** — Docker Compose patterns, dev containers, Tilt/Skaffold for Kubernetes dev loops, Nix for reproducible environments
- `[TEMPLATE]` **Dockerfile templates** — optimized multi-stage builds for each language
- `[TEMPLATE]` **GitHub Actions workflow templates** — lint, test, build, deploy for each language
- `[TEMPLATE]` **docker-compose.yml starters** for common development stacks

### 3.4 Security Patterns

**Deliverables:**

- `[GEN]` **Application security checklist** — OWASP Top 10 mapped to your stack, with concrete mitigations in each language
- `[GEN]` **Authentication architecture guide** — session-based, JWT, OAuth 2.0 + OIDC, passkeys/WebAuthn — implementation tradeoffs
- `[GEN]` **Secrets management guide** — environment variables, Vault, cloud KMS, SOPS, age encryption — with local development patterns
- `[GEN]` **Supply chain security** — dependency pinning, lockfile verification, SBOM generation, vulnerability scanning automation
- `[TEMPLATE]` **Security headers middleware** for Go and TypeScript HTTP servers

---

## Phase 4: Operational Knowledge

*Things you look up every time. Document once, reference forever.*

### 4.1 Runbooks

**Deliverables:**

- `[GEN]` **Database troubleshooting runbook** — slow queries, connection pool exhaustion, lock contention, replication lag, disk space
- `[GEN]` **Container/Kubernetes troubleshooting runbook** — CrashLoopBackOff, OOMKilled, networking issues, DNS resolution, resource limits
- `[GEN]` **Memory leak diagnosis runbook** — per-language tools and techniques (Go pprof, Rust DHAT, Node.js heap snapshots, Python tracemalloc)
- `[GEN]` **Incident response template** — severity classification, communication templates, postmortem structure, follow-up tracking
- `[CAPTURE]` **Project-specific runbooks** from your actual deployments

### 4.2 Checklists

**Deliverables:**

- `[GEN]` **New project bootstrap checklist** — per language: project structure, linting, formatting, testing, CI, documentation, dependency management, license
- `[GEN]` **Code review checklist** — error handling, edge cases, concurrency safety, security, performance, test coverage, documentation
- `[GEN]` **Pre-deployment checklist** — migrations, feature flags, rollback plan, monitoring, alerting, load testing, security scan
- `[GEN]` **Dependency update checklist** — changelog review, breaking changes, security advisories, test coverage, gradual rollout
- `[GEN]` **Post-incident checklist** — evidence preservation, timeline construction, root cause analysis, action items, communication

---

## Phase 5: Cross-Language Comparisons

*The Rosetta Stone section. When you know how to do something in one language, these let you instantly translate to another.*

### 5.1 Pattern Translations

**Deliverables:**

- `[GEN]` **Iterator/stream patterns** — Go (for-range, channels), Rust (Iterator trait), TypeScript (generators, async iterables), Python (generators, itertools)
- `[GEN]` **Builder pattern** — idiomatic implementation in each language (functional options in Go, builder derive in Rust, fluent interfaces in TS, dataclass-based in Python)
- `[GEN]` **Dependency injection** — Go (wire, fx), Rust (manual, shaku), TypeScript (tsyringe, inversify, manual), Python (dependency-injector, manual)
- `[GEN]` **State machine implementations** — enums + match (Rust), iota + switch (Go), discriminated unions (TS), class-based (Python)
- `[GEN]` **CLI argument parsing** — cobra (Go), clap (Rust), commander/yargs (TS), click/typer (Python)

### 5.2 Ecosystem Maps

**Deliverables:**

- `[GEN]` **HTTP framework comparison** — net/http & chi & fiber (Go), axum & actix-web (Rust), Express & Fastify & Hono (TS), FastAPI & Starlette & Django (Python) — with selection criteria
- `[GEN]` **ORM and database driver comparison** — sqlx & GORM (Go), sqlx & diesel & sea-orm (Rust), Prisma & Drizzle & Knex (TS), SQLAlchemy & Tortoise (Python)
- `[GEN]` **Serialization library comparison** — encoding/json (Go), serde (Rust), Zod & io-ts (TS), Pydantic & cattrs (Python)
- `[GEN]` **Logging and observability toolkit comparison** — slog (Go), tracing (Rust), pino & winston (TS), structlog (Python)

---

## Phase 6: Local LLM Setup

*Build your fallback infrastructure for when cloud tokens get expensive.*

### 6.1 Ollama Setup on Apple Silicon

**Deliverables:**

- `[BUILD]` **Install and configure Ollama** — `brew install ollama`, verify Metal acceleration is active
- `[GEN]` **Model selection guide for coding** — benchmarked recommendations:
  - **16GB RAM:** Qwen 2.5 Coder 7B, DeepSeek Coder V2 Lite, CodeLlama 7B
  - **32GB RAM:** Qwen 2.5 Coder 32B, DeepSeek Coder V2 16B, CodeLlama 34B
  - **64GB+ RAM:** DeepSeek Coder V2 33B, Mixtral 8x7B, Llama 3 70B (quantized)
- `[GEN]` **Quantization guide** — Q4_K_M vs. Q5_K_M vs. Q8_0, quality vs. speed tradeoffs on M-series
- `[BUILD]` **Modelfile library** — custom system prompts for different coding tasks (code review, refactoring, documentation, debugging)
- `[GEN]` **Prompt template library** for common coding tasks optimized for smaller models (they need more structured prompts than frontier models)

### 6.2 IDE Integration

**Deliverables:**

- `[BUILD]` **Continue.dev setup** — VS Code extension pointing at local Ollama, custom slash commands, context providers
- `[BUILD]` **Neovim integration** (if applicable) — via `ollama.nvim` or `gen.nvim`
- `[GEN]` **Task routing guide** — which tasks to send to local models vs. cloud models:
  - **Local:** autocomplete, simple refactors, documentation generation, boilerplate, test stubs, regex writing, commit messages
  - **Cloud:** complex architecture decisions, multi-file refactors, novel algorithm design, nuanced code review, debugging subtle concurrency issues
- `[BUILD]` **Shell aliases and scripts** for common LLM tasks — pipe code through local model for quick transformations

### 6.3 Advanced Local Setup

**Deliverables:**

- `[BUILD]` **llama.cpp direct setup** — for when you want maximum control over inference parameters
- `[GEN]` **RAG setup guide** — index your knowledge base for retrieval-augmented generation with local models, using `chromadb` or `qdrant` locally
- `[BUILD]` **Custom tool-use pipeline** — local model + function calling for automated code tasks
- `[GEN]` **Fine-tuning exploration guide** — LoRA/QLoRA on your own code patterns using MLX on Apple Silicon

---

## Phase 7: Workflow Automation

*Replace LLM usage with deterministic tooling wherever possible.*

### 7.1 Code Generation Templates

**Deliverables:**

- `[BUILD]` **Code generators** for each language:
  - Go: `go generate` templates for boilerplate (CRUD handlers, repository interfaces, mock implementations)
  - Rust: proc macros or `cargo-generate` templates for common patterns
  - TypeScript: Plop.js or Hygen generators for components, API routes, test files
  - Python: Cookiecutter templates for project scaffolding
- `[GEN]` **Template library** for each generator — CRUD service, CLI tool, library, API server, worker service

### 7.2 Editor Productivity

**Deliverables:**

- `[GEN]` **Language server mastery guide** — gopls, rust-analyzer, TypeScript language server, Pyright — all the features you're probably not using: code actions, refactoring, unused import cleanup, type inference exploration
- `[GEN]` **Editor snippet library** — VS Code / Neovim snippets for each language covering the patterns in this knowledge base
- `[GEN]` **Linter and formatter configuration** — opinionated configs for: golangci-lint, clippy, ESLint + Prettier, Ruff — with rationale for each rule choice
- `[BUILD]` **Git hooks setup** — pre-commit hooks for formatting, linting, type checking, test running — via `lefthook` or `pre-commit`

### 7.3 Shell and CLI Automation

**Deliverables:**

- `[GEN]` **Makefile/Justfile reference** — portable task runners with common targets: build, test, lint, format, docker, deploy, db-migrate, generate
- `[GEN]` **Shell scripting patterns** — safe bash patterns, error handling, argument parsing, color output, progress indicators
- `[BUILD]` **Personal CLI toolkit** — small scripts for repetitive tasks: project scaffolding, dependency auditing, log searching, port management
- `[TEMPLATE]` **Justfile starters** for each language

---

## Execution Strategy

### Token Budget Allocation (April–June)

Suggested allocation of your remaining cheap tokens:

| Priority | Category | % of Budget | Rationale |
|---|---|---|---|
| 1 | Phase 1 (Cross-cutting patterns) | 30% | Highest reuse value, eliminates the most common repeated questions |
| 2 | Phase 2 (Language deep-dives) | 25% | Deep understanding reduces dependence on LLM for idiomatic code |
| 3 | Phase 3 (System architecture) | 20% | Architectural decisions are the most expensive LLM interactions |
| 4 | Phase 5 (Comparisons) | 10% | Rosetta stone value — learn once, apply across languages |
| 5 | Phase 6 (Local LLM setup) | 10% | Build the fallback infrastructure |
| 6 | Phase 4, 7 (Operations, automation) | 5% | Lower LLM leverage — more hands-on work |

### Daily Workflow

1. **Start of day:** Pick 1–2 deliverables from this plan
2. **Generate:** Use your LLM budget to create the artifact
3. **Edit:** Review, customize, add your own context and project-specific notes
4. **Commit:** Add to your knowledge base repo with a meaningful commit message
5. **Use:** Immediately reference the new artifact in your current project work
6. **Refine:** Update the artifact when you discover gaps or better approaches

### Incremental Value Checkpoints

After completing each phase, you should notice a measurable reduction in certain categories of LLM queries:

- **After Phase 1:** You stop asking "how do I handle errors in X" and "what's the concurrency pattern for Y"
- **After Phase 2:** You stop asking "what's the idiomatic way to do Z in [language]"
- **After Phase 3:** You stop asking "should I use X or Y architecture" — you have decision frameworks
- **After Phase 5:** You stop asking "how do I do [thing I know in Go] in Rust"
- **After Phase 6:** Simple coding tasks no longer require cloud API calls at all
- **After Phase 7:** Boilerplate generation is fully automated, no LLM needed

---

## Appendix A: Architecture Decision Record Template

```markdown
# ADR-NNNN: [Title]

**Date:** YYYY-MM-DD
**Status:** Proposed | Accepted | Superseded by ADR-XXXX | Deprecated

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
|---|---|---|---|
| ... | ... | ... | ... |

## References

- Links to relevant documentation, benchmarks, or discussions
```

## Appendix B: Pattern Documentation Template

```markdown
# [Pattern Name]

**Category:** Concurrency | Error Handling | Data Modeling | API Design | ...
**Languages:** Go | Rust | TypeScript | Python

## Problem

What problem does this pattern solve? When does it arise?

## Solution

Describe the pattern conceptually, independent of language.

## Implementation

### Go
\`\`\`go
// idiomatic implementation
\`\`\`

### Rust
\`\`\`rust
// idiomatic implementation
\`\`\`

### TypeScript
\`\`\`typescript
// idiomatic implementation
\`\`\`

### Python
\`\`\`python
# idiomatic implementation
\`\`\`

## Tradeoffs

- **When to use:** ...
- **When to avoid:** ...
- **Complexity cost:** ...
- **Performance characteristics:** ...

## Related Patterns

- [Link to related pattern]

## Real-World Example

Where have you used this in your own projects? What did you learn?
```

## Appendix C: Snippet Template

```markdown
# [Descriptive Name]

**Language:** Go | Rust | TypeScript | Python
**Category:** HTTP | Database | CLI | Concurrency | ...
**Last Verified:** YYYY-MM-DD
**Dependencies:** list any non-stdlib dependencies

## Usage

When and why you'd reach for this snippet.

## Code

\`\`\`[language]
// the snippet, complete and copy-pasteable
\`\`\`

## Customization Points

- Line N: change X to Y for [different behavior]
- Line M: replace Z with W if [different use case]

## Gotchas

- Things that will bite you if you're not careful
```