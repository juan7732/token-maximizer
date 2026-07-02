# Stack defaults

Decided 2026-07-02 with Juan. The default picks for a new or reworked React repo. Deviate per repo only with a written reason.

| Concern | Default | Why |
|---|---|---|
| Build/runtime | Vite SPA, React, no meta-framework | Local tools and client apps do not need a server runtime; add one when a real need shows up |
| Language | TypeScript, `strict: true` | Non-negotiable for a repo meant to be well-architected |
| Lint/format | ESLint flat config + typescript-eslint + Prettier | Type-aware rules and import-boundary plugins; the boring proven pair |
| Styling | Tailwind v4, tokens via `@theme` | Utility-first, zero runtime cost |
| UI primitives | Hand-rolled on native elements, in `shared/ui` | Owned code over a library; see tenets.md posture note on the a11y cost |
| Server state | Owned fetch hook (~50 lines: AbortController, race-guard, error/loading) | A query library is overkill until cache invalidation pain is demonstrated |
| Client state | Zustand from day one for shared state | 1kb, avoids context re-render traps; local state stays `useState` |
| Forms | react-hook-form + zod | Best field-state ergonomics; validation truth rules below |
| Testing | Vitest + React Testing Library, unit/component only | E2E is opt-in per repo, not a default |

## Forms and validation: the boundary split

When a repo already has a non-zod contract (for example JSON Schema files that a build pipeline and Ajv consume), do not rewrite it and do not duplicate it. Split by boundary:

- The existing contract keeps owning its boundary (content files, build inputs, anything validated server-side or by CLI tooling).
- zod owns app-internal boundaries only: UI state shapes, API response envelopes, form drafts before they are submitted against the real contract.
- The split is documented in the repo's AGENTS.md so no one "unifies" it into duplication.

## The owned fetch hook

The one piece of infrastructure every repo copies in rather than installs. Requirements: AbortController wired to effect cleanup, a race guard so a stale response never overwrites a newer one, explicit loading/error/data states, and JSON parse + schema validation at the edge. Keep it under about 50 lines; if it grows past that, that is the demonstrated pain that justifies a query library.
