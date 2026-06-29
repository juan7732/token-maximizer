# token-maximizer

Incubator and index for a composable, AI-context knowledge base. Drafts live here as flat folders; mature ones graduate into their own repos and rejoin workspaces via ergo. Front-load expensive generation now, harvest the value indefinitely.

The build plan is [knowledge-base-master-plan.md](knowledge-base-master-plan.md).

## Tenets

1. **Incubate flat, graduate to repos.** Content starts as a flat folder here, one per future repo. When ready, it splits into its own repo and comes back as an ergo reference repo. This repo is the incubator and index; mature knowledge lives in focused repos.
2. **Reference, not submodules.** ergo is the composition layer. Graduated repos join workspaces as reference repos, never git submodules, avoiding pinned-SHA and detached-head friction.
3. **Multi-repo, not a monolith.** A context layer you inhabit, not a place you visit. Repos are pulled selectively into project workspaces so AI sees only relevant knowledge.
4. **Front-load generation.** Spend tokens on expensive generation now; reuse forever. Tag work `[GEN]` `[BUILD]` `[CAPTURE]` `[TEMPLATE]`.
5. **Create repos on graduation, not up front.** GitHub repos are made when a folder can stand on its own.
6. **Token-efficient by default.** Capture the content in full but cut verbiage. Simple, dense, concise.

## Structure

- `knowledge-base-master-plan.md` the build plan and phases
- `ergo/` ergo usage and project-creation docs
- `common/` cross-cutting reference (gh snippets)
- `tenets/` ergo design tenets
- `__archive/` retired drafts and specs
