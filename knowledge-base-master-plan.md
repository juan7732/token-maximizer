# Architectural Knowledge Base: Master Plan

> **Purpose:** Build a durable, composable knowledge base as a set of repos that integrate
> into any project workspace via ergo, providing ambient AI context and reducing LLM
> dependency over time.
> **Core Languages:** Go · Rust · TypeScript · Python
> **Architecture:** Multi-repo, composed via ergo workspaces. Not a monolith you visit — a
> context layer you inhabit.
> **Token Strategy:** Front-load expensive generation now; harvest value indefinitely.

This master plan supersedes the earlier v1 and v2 drafts. Where they disagreed, v2 wins:
the knowledge base is many composable repos, not one repo with subdirectories. v1's deep
content catalog (Phases 2–7) is preserved below, remapped onto the multi-repo layout.

---

## How to Use This Document

Each phase contains **modules** with concrete **deliverables** — artifacts you generate
and commit to the appropriate repo. You don't need to complete phases sequentially. Pick
the modules most relevant to your current projects and work outward. The numbering reflects
dependency order, not priority.

Every deliverable is tagged:

- `[GEN]` — Generate with LLM assistance (spend tokens on these now)
- `[BUILD]` — Hands-on implementation work (light LLM use)
- `[CAPTURE]` — Extract patterns from your actual project work into the knowledge base
- `[TEMPLATE]` — Reusable boilerplate you'll copy-paste forever

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
its own. Create the GitHub repos as folders graduate, not all up front.

---

## Phase 0: Foundation — The Repo Architecture

### 0.1 Repo Topology

The knowledge base graduates into these repos, each serving a distinct role:

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

| Repo | Why separate | When you'd pull it in |
|------|-------------|----------------------|
| `kb-core` | Always relevant, language-agnostic | Every workspace |
| `kb-go` | Only useful when writing Go | Go project workspaces |
| `kb-rust` | Only useful when writing Rust | Rust project workspaces |
| `kb-typescript` | Only useful when writing TypeScript | TS project workspaces |
| `kb-python` | Only useful when writing Python | Python/ML workspaces |
| `kb-templates` | Structural scaffolding, not reference | When starting new docs/patterns |
| `kb-ops` | Operational, not development | Incident response, deployments |
| `kb-comparisons` | Reference when translating between languages | Multi-language workspaces |

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

- `[BUILD]` Stub each kb domain as a flat folder in this repo; graduate to a GitHub repo when ready
- `[GEN]` Generate README.md for each repo: purpose, structure, index of contents, when to pull into a workspace
- `[GEN]` Generate all template files in `kb-templates` (see §0.4)
- `[BUILD]` Create the knowledge-base ergo workspace TOML (see §0.5)
- `[BUILD]` Create ergo workspace TOMLs for active projects that include relevant kb repos

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
|---|---|---|---|
| ... | ... | ... | ... |

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

- `[GEN]` **philosophy.md** — The spectrum from exceptions to Result types to error values. Not syntax — *when* to use which strategy and *why*. Decision framework based on project type, team size, and reliability requirements.
- `[GEN]` **taxonomy.md** — Reusable error classification: transient vs. permanent, retryable vs. fatal, user-facing vs. internal, expected vs. unexpected. Includes a template for project-specific taxonomies.
- `[GEN]` **propagation.md** — How errors flow through layers (handler → service → repository → external) with diagrams. Wrapping, context enrichment, boundary translation.

Per-language implementation goes in `kb-<lang>/idioms/error-handling.md`:
- `[GEN]` **Go:** sentinel errors, custom error types, `errors.Is`/`errors.As`, wrapping, panic conventions
- `[GEN]` **Rust:** `Result<T, E>`, `thiserror` vs. `anyhow`, `?` operator, custom error enums, `From` impls
- `[GEN]` **TypeScript:** discriminated unions, `neverthrow`, error boundaries, Zod validation errors
- `[GEN]` **Python:** exception hierarchies, `@contextmanager`, structured logging, `ExceptionGroup`

### 1.2 Concurrency (`kb-core/patterns/concurrency/`)

- `[GEN]` **mental-models.md** — threads vs. green threads vs. async, shared state vs. message passing, structured concurrency, backpressure, cancellation
- `[GEN]` **patterns.md** — fan-out/fan-in, pipeline, worker pool, pub/sub, rate limiter, circuit breaker, with idiomatic implementations across all four languages (code in `kb-<lang>/snippets/`)
- `[GEN]` **debugging.md** — deadlock/race condition symptoms, tools, prevention
- Per-language: Go (goroutines, channels, `errgroup`, leaks), Rust (`tokio`, `Arc<Mutex>`, `Send`/`Sync`, `rayon`), TS (event loop, `Promise.all*`, workers, `AbortController`), Python (`asyncio`, GIL, `multiprocessing`, `trio`)

### 1.3 API Design (`kb-core/patterns/api-design/`)

- `[GEN]` REST checklist (naming, versioning, pagination, idempotency); gRPC guide; GraphQL decision framework; authN/authZ patterns (JWT, OAuth2, mTLS, RBAC vs ABAC)
- `[TEMPLATE]` OpenAPI 3.1 starter, protobuf service starter

### 1.4 Data Modeling (`kb-core/patterns/data-modeling/`)

- `[GEN]` modeling decision tree, schema evolution, per-language validation (Go tags, serde+validator, Zod, Pydantic), serialization comparison

### 1.5 Testing Strategy (`kb-core/patterns/testing-strategy/`)

- `[GEN]` testing pyramid, test doubles taxonomy, integration isolation; per-language toolkits go in `kb-<lang>/testing/`
- `[TEMPLATE]` test file starters per language

---

## Phase 2: Language-Specific Deep Dives (`kb-go`, `kb-rust`, `kb-typescript`, `kb-python`)

### 2.1 Go
- `[GEN]` idioms compendium, concurrency cookbook, performance guide, project layout, generics patterns
- `[CAPTURE]` lessons learned from Go projects

### 2.2 Rust
- `[GEN]` ownership mental model, trait design patterns, error design playbook, async deep-dive, FFI/unsafe, performance, cargo ecosystem map

### 2.3 TypeScript
- `[GEN]` type system deep-dive, patterns for scale, runtime vs compile-time safety, Node ops, frontend architecture, performance

### 2.4 Python
- `[GEN]` modern patterns, packaging/deps, async patterns, systems programming (PyO3), CLI tooling

---

## Phase 3: System Architecture (`kb-core/architecture/`)

- **3.1 Service:** decision framework, communication patterns, decomposition, observability; `[TEMPLATE]` service skeletons (Go, Rust)
- **3.2 Data:** database selection, caching, event sourcing/CQRS, pipelines, migrations
- **3.3 Infra:** containers, CI/CD, IaC comparison, local dev; `[TEMPLATE]` Dockerfiles, GH Actions, compose stacks
- **3.4 Security:** OWASP checklist, authN architecture, secrets, supply chain; `[TEMPLATE]` security headers middleware

---

## Phase 4: Operational Knowledge (`kb-ops`)

- **4.1 Runbooks:** database, container/k8s, memory leak diagnosis, incident response; `[CAPTURE]` project runbooks
- **4.2 Checklists:** project bootstrap, code review, pre-deployment, dependency update, post-incident

---

## Phase 5: Cross-Language Comparisons (`kb-comparisons`)

- **5.1 Pattern translations:** iterators/streams, builder, DI, state machines, CLI parsing
- **5.2 Ecosystem maps:** HTTP frameworks, ORMs/drivers, serialization, logging/observability

---

## Phase 6: Local LLM Setup (`kb-ops` or dedicated `kb-local-models`)

- **6.1 Ollama on Apple Silicon:** install, model selection by RAM, quantization, Modelfile library
- **6.2 IDE integration:** Continue.dev, Neovim, task routing local vs cloud, shell aliases
- **6.3 Advanced:** llama.cpp, RAG over the knowledge base, tool-use pipeline, LoRA/QLoRA via MLX

---

## Phase 7: Workflow Automation (`kb-templates`, `kb-ops`)

- **7.1 Code generation:** per-language generators, template library
- **7.2 Editor productivity:** language server mastery, snippet library, linter configs, git hooks
- **7.3 Shell/CLI:** Makefile/Justfile reference, bash patterns, personal CLI toolkit; `[TEMPLATE]` Justfile starters
