# AGENTS.md

Conventions for agents and contributors. Keep it boring and consistent.

## What this repo is

Incubator and index for a composable knowledge base. Drafts live as flat folders, one per future repo. When a folder is ready it graduates into its own repo and rejoins workspaces via ergo. The plan is `knowledge-base-master-plan.md`.

## Tenets (non-negotiable)

1. Incubate flat, graduate to repos. One folder per future repo; split out when it can stand on its own.
2. Reference repos via ergo, never git submodules.
3. Multi-repo composed by ergo, not a monolith. Pull only relevant knowledge into a workspace.
4. Front-load expensive generation, harvest forever. Tag work `[GEN] [BUILD] [CAPTURE] [TEMPLATE]`.
5. Create GitHub repos on graduation, not up front.
6. Token-efficient by default. Capture the content in full but cut verbiage. Simple, dense, concise. Bullets over prose, no filler.

## Where things live

- `knowledge-base-master-plan.md` build plan and phases (supersedes prior v1/v2 drafts)
- `ergo/` ergo usage and project-creation docs
- `common/` cross-cutting reference
- `tenets/` ergo design tenets
- `__archive/` retired drafts and specs

## Conventions

- Markdown only. No invented metrics. No em-dashes.
- Write tight: keep substance, drop filler. Prefer bullets and tables over paragraphs.
- Plain prose. No "it's not just X, it's Y" constructions or other AI-typical contrastive filler.
- Code evidence (file:line) must pin its source commit in the file header (`Source: repo @ hash`). If the source cannot be pinned or will not be available to consuming workspaces, drop the evidence rather than misguide a future agent.
- Captured content beats generated content. A capture earns its place only if it is opinionated, non-obvious to a strong model, and transferable (see `kb-templates/capture.md`).
- When writing code (snippets, templates, tooling), mark judgment calls with the `DECISION/REVIEW/SPEC/TODO` comment taxonomy (see `kb-agent-practices/decision-hygiene.md`).
- A folder maps to one future repo; keep it self-contained so it can graduate cleanly.
- Don't create the GitHub repo until the folder is ready to stand alone.
