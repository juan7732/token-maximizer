# Decision Hygiene

**Category:** Agent Practices · **Source:** `github.com/juan7732/ergo` @ `6b9b22c` · **Captured:** 2026-07-01

Practices that keep judgment calls visible when an agent writes most of the code. Proven in the ergo build (Opus orchestrating, Sonnet coding).

## Greppable decision taxonomy

Mark every judgment call the spec did not force, with a fixed comment vocabulary:

- `// DECISION:` intentional choice among plausible alternatives, with the reason
- `// REVIEW:` unsure, a human should check this
- `// SPEC:` cites the spec section that forced the code
- `// TODO:` known gap

`rg "// (DECISION|REVIEW):"` then lists every judgment call in the codebase. Review becomes targeted: read the REVIEWs first, audit the DECISIONs, trust the SPEC-cited code to the spec.

Why it works with agents: an agent filling spec gaps makes silent choices constantly. The taxonomy converts those into artifacts you can find, so intent survives the session that produced it. Instruct the agent to use the taxonomy in AGENTS.md and it self-reports its own judgment calls.

Evidence at the pinned commit: `internal/workspace/resolve.go:64`, `internal/config/validate.go:66`, `cmd/update.go:93`, `cmd/open.go:87`.

## Tenets cross-linked to code

Keep a checked-in doc that maps each design tenet to the code embodying it (ergo: `ergo-docs/09-design-tenets.md`). Tenets without code pointers drift into aspiration. The doc gives an agent a compact answer to "how does this project want things done", with proof it can read.

Caution: cross-repo relative links rot. ergo's tenets doc points at `../../token-maximizer/tenets.md`, which has since moved. Pin links to a commit or use repo-qualified references.

## Capture unit

The atomic unit of capture is one DECISION comment, not "a lesson learned". Writing it costs seconds at the moment of choice. Harvesting into a kb is a later, mechanical pass (see `distillation.md`).
