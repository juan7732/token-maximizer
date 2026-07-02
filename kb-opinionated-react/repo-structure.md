# Repo structure

Decided 2026-07-02 with Juan.

## Workspace shape

When a front-end lives alongside non-FE tooling (a build pipeline, a CLI, shared schemas), use npm workspaces in one repo. The app gets its own `package.json`, deps, and scripts; it stays colocated with the system it serves so shared-library seams stay honest and importable. Extract to a separate repo only when a second consumer actually exists.

```
repo/
  package.json          # workspaces root, no FE deps here
  <domain dirs>/        # build/, schema/, content/, whatever the repo is about
  app/
    package.json        # FE deps live here and nowhere else
    src/
```

## Source layout: feature folders + shared core

```
src/
  features/
    <feature>/          # components, hooks, api calls, tests, all colocated
  shared/
    ui/                 # hand-rolled primitives (Button, Dialog, Field)
    lib/                # owned infrastructure: fetch hook, formatting, storage
    api/                # API client + response schemas
  app/                  # entry, providers, top-level shell
```

Rules, all lint-enforced (tenets.md #2):

- A feature never imports from another feature. Shared code moves to `shared/` first.
- `shared/` never imports from `features/`.
- Cross-cutting types and schemas live with the boundary that owns them (`shared/api` for network shapes), never in a grab-bag `types/`.

## What stays out

- No flat `components/`, `hooks/`, `utils/` top level. By-type layout encodes no boundaries and every rule above becomes unenforceable.
- No `src/types/` dumping ground; it decays into a second, hand-maintained truth (tenets.md #3).
- No barrel files (`index.ts` re-export hubs) beyond a feature's single public entry; they hide the import graph the lint rules need to see.
