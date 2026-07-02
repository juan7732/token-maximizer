# Decisions Harvested from ergo

**Category:** Decisions · **Source:** `github.com/juan7732/ergo` @ `6b9b22c` · **Captured:** 2026-07-02

Harvested via `rg "// DECISION:"`. Each entry is the decision as recorded in code, generalized where it transfers. `// REVIEW:` comments were excluded; they are open questions, not decisions.

## Package-manager-owned binaries must not self-replace

**Context:** `ergo update` self-updates the binary, but the binary may have been installed via Homebrew.
**Decision:** detect when the resolved binary lives under a Homebrew prefix and refuse, pointing at `brew upgrade` instead. Only the standalone release-download install path self-updates.
**Why:** brew owns the file under its Cellar; a hand-swapped binary gets clobbered on the next `brew upgrade`. Fighting the package manager loses.
**Transfers to:** any self-updating tool distributed through both a package manager and direct download.
Evidence: `cmd/update.go:93`.

## Empty string means not provided

**Context:** TOML has no set-but-empty distinction for string keys; `group = ""` and no `group` key decode identically.
**Decision:** `group = ""` means "no group" and is valid, not a validation error.
**Why:** validating a distinction the format cannot express creates errors users cannot reason about.
**Transfers to:** any config format without set-vs-empty distinction; decide the semantics once and record them.
Evidence: `internal/config/validate.go:66`.

## Unique partial match resolves without asking

**Context:** commands accept a partial workspace name that may match zero, one, or many workspaces.
**Decision:** a single unambiguous partial match resolves directly; multiple matches return candidates for the picker.
**Why:** mirrors git's unique branch-prefix behavior; asking for confirmation of an unambiguous answer wastes the user's time.
**Transfers to:** any CLI name resolution; pair with a deterministic resolver returning `Name | Candidates` (see `kb-opinionated-cli/patterns-from-ergo.md`).
Evidence: `internal/workspace/resolve.go:64`.

## On conflicting selections, match the positional grammar

**Context:** the TUI group/tag selector can end up with both groups and tags selected, but `ergo show` takes one group positionally.
**Decision:** group takes precedence, first selected group wins.
**Why:** the interactive path should produce the same shape of answer as the non-interactive grammar, so TUI results are expressible as flags.
**Transfers to:** any TUI fallback for a flag-driven command; the TUI's output must round-trip to the command line.
Evidence: `internal/tui/group_select.go:137`.
