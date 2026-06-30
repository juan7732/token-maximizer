# Creating New Projects with ergo + gh

> **Purpose:** A pragmatic playbook for spinning up new projects on GitHub
> using `gh` to create repos and `ergo` to compose them into a workspace
> optimized for AI-assisted development.
>
> **Audience:** Solo developer, personal GitHub account, macOS.
> **Companion docs:** [ergo-usage.md](ergo-usage.md), [gh-snippets.md](gh-snippets.md), [tenets.md](tenets.md).

---

## Core philosophy

**Repos are the unit of scope.** A repo is a release boundary, a CI boundary,
a permissions boundary, and - most importantly for AI workflows - a
**context boundary**. Smaller, focused repos give an AI agent a tractable
surface area. Mega-monorepos drown agents in irrelevant code.

**Workspaces are the unit of work.** An ergo workspace stitches related repos
together into a single VS Code session. You get monorepo-like ergonomics
(one window, cross-repo search, one command runner) without monorepo
governance overhead.

**The split is a design decision, not a default.** Always start by asking:
*what are the natural seams in this system?* Then make each seam a repo.

---

## When to split, when to keep together

### Split into separate repos when…

- **Different release cadence.** A CLI ships weekly; its docs site ships
  on every commit. Separate repos, separate pipelines.
- **Different runtime / language.** Go service + TypeScript frontend +
  Python ML notebooks. Each has its own toolchain, lockfile, CI.
- **Different audience / visibility.** Public SDK + private infra. You
  cannot mix these in one repo without leaking.
- **Reusable across projects.** A shared `kb-go` patterns library used by
  three CLIs belongs in its own repo.
- **Independent ownership / contribution model.** Even solo, if you'd
  accept PRs to one but not the other.
- **Useful as an AI context unit on its own.** If you'd ever want an agent
  scoped *only* to this code, it should be its own repo.

### Keep in one repo when…

- The code is **inseparable** - one binary, one deploy, one test suite.
- Splitting would force you to invent a coordination protocol (versioned
  internal API, release-pinning dance) for code that's actually coupled.
- The "second repo" is just a folder of helpers used in exactly one place.

> **Rule of thumb:** if you can't write a one-sentence purpose for a repo
> that's distinct from its neighbors, merge it.

---

## Common project archetypes

### 1. CLI + docs site

```
my-tool/                  # the CLI itself (Go/Rust/etc.)
my-tool-docs/             # marketing site / docs (Astro, Next.js, etc.)
```

Why split: docs deploy to Cloudflare Pages on every commit; CLI releases
on tags via goreleaser. Different velocities, different toolchains.

### 2. Service + client SDK(s)

```
foo-api/                  # backend service
foo-sdk-ts/               # TypeScript client
foo-sdk-go/               # Go client
foo-examples/             # runnable examples consuming the SDKs
```

Why split: SDKs version independently of the API. Each SDK has its own
package ecosystem (`npm`, Go modules). `foo-examples` is a sandbox you
don't want polluting SDK repos.

### 3. Web app

```
my-app-web/               # frontend (Next.js / Vite)
my-app-api/               # backend (Go/Node)
my-app-infra/             # terraform / wrangler config / migrations
```

Why split: deploy targets differ (Cloudflare Pages, Fly.io, etc.). Infra
changes need review independently of feature code.

### 4. Research / ML project

```
proj-data/                # data prep + dataset cards (private)
proj-experiments/         # notebooks, training runs
proj-model/               # production inference code / model card
proj-paper/               # writeup, figures
```

Why split: data is private and large; experiments are messy; model is the
shipping artifact. Each has different review and retention needs.

### 5. Knowledge base / patterns library

```
kb-core/                  # cross-cutting patterns and architecture
kb-go/                    # Go-specific idioms
kb-rust/                  # Rust idioms
kb-templates/             # starter templates
```

Why split: each domain evolves independently and is referenced
independently by other projects (and by AI agents scoped to one language).

---

## The playbook

### Step 1 - Sketch the seams

Before typing anything, write down (in a scratch file or just out loud):

1. What ships? (the deployable artifacts)
2. Who/what consumes each? (humans, services, agents)
3. What changes together? What changes independently?
4. Where are the trust boundaries? (public/private, secrets)

Each distinct answer is a candidate repo. Collapse aggressively if the
distinction is theoretical rather than concrete.

### Step 2 - Create the repos on GitHub

Use `gh` to batch-create. Naming convention: `<project>-<role>` so they
sort together and the role is obvious.

```bash
PROJECT=foo
for role in api sdk-ts sdk-go examples; do
  gh repo create "${PROJECT}-${role}" --public \
    --description "${PROJECT}: ${role}" \
    --add-readme=false
done
```

Tips:
- Use `--public` unless there's a real reason for private. Personal repos
  benefit from being indexable.
- Skip `--clone` here - let ergo handle materialization.
- Skip `--add-readme`; you'll commit a real one in step 4.
- Apply topics so they're discoverable as a set:
  ```bash
  for role in api sdk-ts sdk-go examples; do
    gh repo edit "${PROJECT}-${role}" --add-topic "${PROJECT},project:${PROJECT}"
  done
  ```

### Step 3 - Define the ergo workspace

```bash
ergo init foo
```

In the TUI, add each repo by its clone URL and assign:

- A **group** per logical layer (`backend`, `clients`, `examples`,
  `infra`). Groups are the primary filter for `ergo show` and `ergo run`.
- **Tags** for cross-cutting concerns (`go`, `typescript`, `deploys`,
  `ai-context`). Tags are any-match, so they compose well.
- A **non-repo folder** for ad-hoc notes the project needs but that
  doesn't deserve its own repo:
  ```bash
  ergo add folder scratch
  ergo add folder ai-notes --git   # versioned context for agents
  ```

Then materialize:

```bash
ergo open foo
```

This clones everything, generates `.code-workspace`, and opens VS Code.

### Step 4 - Seed each repo

Inside the workspace, scaffold each repo with the bare minimum:

```bash
ergo run --group=backend -- bash -c '
  [[ -f README.md ]] || echo "# $(basename $PWD)" > README.md
  [[ -f .gitignore ]] || curl -sL "https://www.toptal.com/developers/gitignore/api/go,macos" > .gitignore
'
```

Or, for one-off setup that needs language-specific tooling:

```bash
ergo run --tags=go -- go mod init "github.com/$(gh api user --jq .login)/$(basename $PWD)"
ergo run --tags=typescript -- npm init -y
```

Commit and push the scaffolding in one sweep:

```bash
ergo run -- git add -A
ergo run -- git commit -m "chore: initial scaffolding"
ergo run -- git push -u origin main
```

### Step 5 - Give each repo a `.ergo/` command surface

The single most useful convention for `ergo run` is **uniform command
names across heterogeneous repos**. If every repo answers to `just build`,
`just test`, and `just run`, then `ergo run -- just test` does the right
thing everywhere - even when one repo is Go, another is TypeScript, and a
third is a Python notebook environment.

Give every repo a `.ergo/` folder containing a `justfile` (or `Makefile`,
or shell scripts - pick one and stay consistent) with the repo-local
recipe for each verb:

```
foo-api/.ergo/justfile        # build → go build ./...   test → go test ./...
foo-sdk-ts/.ergo/justfile     # build → pnpm build       test → pnpm test
foo-examples/.ergo/justfile   # build → noop             run → python main.py
```

Example `.ergo/justfile` for a Go repo:

```just
set working-directory := '..'

build:
    go build ./...

test:
    go test ./...

run *args:
    go run . {{args}}

lint:
    go vet ./...
```

Then across the workspace:

```bash
ergo run -- just --justfile .ergo/justfile build
ergo run -- just --justfile .ergo/justfile test
```

Or, simpler - put a top-level `justfile` in each repo that delegates to
`.ergo/`, so the workspace command stays clean:

```bash
ergo run -- just build
ergo run -- just test
```

Why `.ergo/` and not the repo root? It keeps the workspace-oriented
recipes separate from whatever build tooling the repo already uses for
its own developers, and it signals "this exists to be invoked by ergo."
Repos without an `.ergo/` folder simply opt out; `ergo run` skips repos
where the command fails (use `--fail-fast` only when you want strict
behavior).

### Step 6 - Set up AI context (the multiplier)

This is where the multi-repo + ergo combination pays off. The workspace
gives an AI agent **exactly one window** with **exactly the repos it
needs**, no more.

**Per-repo `.github/copilot-instructions.md`.** Short, repo-specific.
What is this repo? What conventions matter here? See the `ergo` repo's
own file for a template.

**Project-level context in a dedicated folder.**

```bash
ergo add folder ai-notes --git
```

Inside `ai-notes/`, keep:
- `architecture.md` - how the repos relate, the data flow, the deploy story.
- `decisions/` - one ADR per real decision.
- `prompts/` - reusable prompt scaffolds (e.g. "review a PR in this codebase").
- `glossary.md` - domain terms the agent should know.

**Use `ergo show` to scope agent sessions.** When you ask an agent to
work on the SDK, run `ergo show clients` first. The agent sees only SDK
repos; cross-repo search results stay clean.

**Use `ergo run` to ground agents in reality.** Instead of letting an
agent guess at state, run a command across the workspace and feed the
output:

```bash
ergo run -s -- git status              # what's in flight
ergo run --tags=go -- go vet ./...     # lint everything
ergo run -- gh pr status               # open PRs across the project
```

### Step 7 - Document the project inside `ai-notes/`

Drop a `README.md` inside `ai-notes/` that describes the *project as a
whole* - what each repo is, how they fit together, and the common
workspace-level commands. Each repo's own README stays scoped to that
repo only; the workspace-level view lives with the rest of the agent
context.

```markdown
# foo

Multi-repo project. Open with `ergo open foo`.

## Repos
- `foo-api` - backend service (Go)
- `foo-sdk-ts` - TypeScript client
- `foo-sdk-go` - Go client
- `foo-examples` - runnable demos

## Common tasks
- Build everything: `ergo run -- just build`
- Test everything: `ergo run -- just test`
- Status snapshot: `ergo status -s`
- Cut releases: `ergo run --tags=releasable -- gh release create ...`
```

This pairs naturally with the rest of `ai-notes/` (architecture, ADRs,
prompts, glossary) so an agent gets the whole picture from one folder.

---

## Reference repos - bringing the world into your workspace

One of ergo's quietest superpowers is dropping **third-party repos** into
a workspace as read-only context. This is invaluable when:

- You're researching how a library/framework actually works and want the
  agent to read the source, not hallucinate from training data.
- You're integrating with an SDK and want canonical examples local and
  searchable.
- You're doing comparative research across several projects in the same
  space.
- You're working from a paper's reference implementation.

### Adding reference repos

Use a dedicated `reference` group and a `reference` tag so they're easy
to exclude from cross-cutting commands:

```bash
ergo add repo https://github.com/charmbracelet/bubbletea.git \
  --group=reference --tags=reference,go
ergo add repo https://github.com/spf13/cobra.git \
  --group=reference --tags=reference,go
ergo sync
```

Then set `excluded_groups = ["reference"]` under `[run]` in
`~/.ergo/config.toml` so `ergo run -- git pull` and friends don't touch
them by default. Override on demand with `--include-group=reference` or
`--all`.

### Scoping the agent to reference + your code

```bash
ergo show reference         # just the reference repos
ergo show backend           # back to your own code
ergo show all               # everything (default)
```

A common flow:

1. `ergo show reference` and ask the agent to summarize the relevant
   patterns in the upstream code.
2. Capture the findings in `ai-notes/references/<lib>.md`.
3. `ergo show backend` and apply them.

### Pruning reference repos

Reference repos are disposable by design. When you're done:

```bash
ergo remove repo bubbletea --force   # also wipes the local clone
```

The TOML entry and the on-disk clone go away; the upstream repo is
untouched. Re-add anytime with `ergo add repo <url>`.

> **Tip:** keep a `ai-notes/references/INDEX.md` listing which reference
> repos are currently checked out and why. Reference repos drift in and
> out - the index gives future-you (and any agent session) a fast answer
> to "why is this here?"

---

## Repo-splitting heuristics for AI workflows

The "context boundary" lens deserves its own treatment:

| Scenario                                                                               | Heuristic                                                    |
| -------------------------------------------------------------------------------------- | ------------------------------------------------------------ |
| Agent needs to read all code to make sense of anything →                               | Probably one repo.                                           |
| Agent does great in one part but gets confused crossing into another →                 | Split at the seam.                                           |
| You routinely tell an agent "ignore the `infra/` folder" →                             | `infra/` should be its own repo, scoped out via `ergo show`. |
| Two areas have different code styles / linters / test frameworks →                     | Split. The conflicting conventions confuse agents too.       |
| You're scared to let an agent touch one folder (prod infra, secrets, generated code) → | Split, and don't include in the agent's workspace view.      |

---

## Lifecycle commands cheat sheet

```bash
# Birth
gh repo create foo-api --public --description "..."
ergo init foo
ergo open foo

# Day-to-day
ergo status -s                       # what's dirty
ergo sync                            # pull everything
ergo run -- just test                # test across repos (via .ergo/ recipes)

# Add a new repo to an in-flight project
gh repo create foo-cron --public
ergo add repo git@github.com:$(gh api user --jq .login)/foo-cron.git \
  --group=backend --tags=go
ergo sync                            # materialize

# Cut releases
ergo run --tags=releasable -- gh release create v0.1.0 --generate-notes

# Retire a repo
ergo remove repo foo-cron            # from TOML
gh repo delete owner/foo-cron --yes  # from GitHub (irreversible)
```

---

## Anti-patterns

- **Monorepo by accident.** Putting unrelated tools in one repo because
  "it's faster." It's faster for five minutes and slower forever.
- **Microrepo theater.** Splitting one cohesive package into four because
  it "feels modular." If they always change together, they're one thing.
- **Skipping the workspace.** Cloning repos manually and opening them as
  separate VS Code windows. You lose cross-repo search, shared run
  commands, and any chance of giving an agent a coherent view.
- **Letting the workspace bloat.** If a workspace has 30 repos and you
  only ever touch 6 at a time, split it (or use `ergo show` aggressively).
- **No AI context folder.** Agents end up rediscovering project structure
  every session. A 100-line `ai-notes/architecture.md` saves hours.
- **Per-repo READMEs that try to explain the whole project.** Keep
  per-repo READMEs scoped to that repo; put project-level docs in the
  workspace root or a dedicated `*-docs` repo.

---

## Quick reference - full bootstrap

```bash
PROJECT=foo
ROLES=(api sdk-ts sdk-go examples)
USER=$(gh api user --jq .login)

# 1. Create repos
for role in "${ROLES[@]}"; do
  gh repo create "${PROJECT}-${role}" --public \
    --description "${PROJECT}: ${role}" \
    --add-topic "${PROJECT}"
done

# 2. Define workspace
ergo init "${PROJECT}"   # add repos in the TUI, group by layer

# 3. Materialize
ergo open "${PROJECT}"

# 4. Seed AI context
ergo add folder ai-notes --git
echo "# ${PROJECT} architecture" > ai-notes/architecture.md

# 5. Confirm
ergo status -s
ergo run -- gh repo view --json name,visibility --jq '"\(.name) \(.visibility)"'
```
