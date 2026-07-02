# Tenets

Decided 2026-07-02 with Juan. These anchor every front-end repo; the other kb files are applications of them.

## 1. Justify every dependency

A dependency must beat roughly 50 lines of owned code to earn its place. Each repo names its deps and why they are there (README or a DEPS section). Corollary defaults that follow from this: data fetching is a small owned hook (fetch + AbortController), not a query library; UI primitives are hand-rolled on native elements, not a component library. Where a dep is worth it, take it without guilt: react-hook-form and zod for forms, zustand for shared client state.

## 2. Boundaries are lint-enforced

Architecture that lives only in prose erodes. Every structural rule gets a machine check: no cross-feature imports, `shared/` never imports from `features/`, server code never imports component code. ESLint flat config with `typescript-eslint` and an import-boundary rule set is the enforcement layer. If a boundary cannot be expressed as a lint rule, question whether it is real.

## 3. Typesafe end-to-end, one truth per boundary

No hand-maintained parallel types. Types derive from the schema that owns each boundary, and runtime validation runs at every trust boundary: network responses, storage reads, user input. Two validation libraries may coexist only when each owns a distinct boundary and the split is documented (see forms-and-validation notes in stack-defaults.md). Within one boundary, one truth.

## Posture notes

- Bias to light and simple. Plain Vite SPA until a real routing or server need shows up. Boring, proven tools over novel ones.
- Owning primitives means owning the hard 10 percent: focus traps, portals, keyboard handling, ARIA. Budget for it when hand-rolling; it is part of a component being done, not polish.
- Testing earns its keep at the unit and component level (Vitest + Testing Library). E2E harnesses are opt-in per repo when a flow is risky enough to pay for one.
