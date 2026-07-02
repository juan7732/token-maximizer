# CLI Patterns from ergo

**Category:** CLI Design · **Source:** `github.com/juan7732/ergo` @ `6b9b22c` · **Captured:** 2026-07-01

Distilled from a real codebase. These are defaults with reasons; deviate when the reason does not apply. Evidence paths are valid at the pinned commit.

## Derived artifacts

**Stamp identity into generated files.** Host formats that ignore unknown keys can carry your metadata. ergo embeds a top-level `"ergo"` key (workspace name, view filter) in the generated `.code-workspace`, so detecting "which workspace am I in" is walking up directories looking for that key. No registry, no sidecar dotfile. Transient UI state lives in the derived file; the source-of-truth TOML stays clean.
Evidence: `internal/vscode/generate.go:19`, `internal/workspace/detect.go:76`.

**Write-if-changed regeneration.** Deterministic generator, then a byte-equality gate before writing. Report `updated` or `unchanged`. No-op writes fire file watchers (VS Code restarts language servers on them). Canonical JSON with a trailing newline keeps diffs clean.
Instead of: rewriting on every run, or comparing mtimes.
Evidence: `internal/vscode/diff.go:15`.

## Destructive safety

**Filters never widen or narrow destruction.** When a command has filters (`--group`, `--tags`) and a destructive mode (`--force`), compute the destruction scope from the full config, passed explicitly (`KnownNames`), never from the filtered view. Otherwise `sync --group=x --force` deletes repos that fell outside the filter. ergo treats this as a named bug class with a regression test. Declare contradictory flags with `MarkFlagsMutuallyExclusive` rather than ad hoc checks.
Evidence: `internal/workspace/manager.go:75`, `cmd/sync.go:43`, `test/integration/sync_filter_test.go`.

**Disposable state cache.** Write the cache contract first: performance only, correctness never depends on it. Every read failure (missing, unreadable, corrupt) returns a zero value and nil error, then self-heals on the next save. No schema versioning, no migrations, no errors the user cannot act on.
Evidence: `internal/workspace/state.go:36`.

## Fan-out over user items

**Errors in results; errgroup for lifecycle only.** For fan-out where partial failure is normal (multi-repo clone, deploy, lint): pre-allocate `[]Result` indexed by position so output preserves declaration order, bound concurrency with a buffered-channel semaphore, store per-item failure in `results[i].Err`, and always return nil from the goroutine. `Wait()` then only surfaces infrastructure errors. Split `ExitCode` (their command failed) from `Err` (our plumbing failed).
Instead of: idiomatic errgroup, where the first error cancels the whole batch.
Evidence: `internal/workspace/manager.go:159`, `internal/workspace/runner.go:23`.

## Interaction design

**TUI as fallback.** Resolve deterministically first: exact match, unique prefix (like git branches), `.` shorthand, CWD detection. Only genuine ambiguity launches a picker, pre-filtered by the partial arg. Shape: a pure resolver returning `Name | Candidates`, and a thin TUI that consumes only the Candidates branch. The same invocation works in scripts and by hand. Render inline, no alt-screen, so short flows stay in scrollback.
Instead of: usage errors on missing args, or a separate interactive mode.
Evidence: `internal/workspace/resolve.go:24`, `cmd/validate.go:125`.

**Stdout as capture contract.** A flag whose output feeds `$(...)` makes stdout a one-value channel: route all narration to stderr while it is active, and ship the shell wrapper in the help text. ergo: `open --print-dir` exists for `cd "$(ergo open --print-dir foo)"`.
Evidence: `cmd/open.go:96`.

## Abstractions

**One-method exec seam.** Shell out to git/gh/code rather than importing go-git or an API client; you inherit the user's auth and skip the protocol surface. The only abstractions are test seams: a single-method `Runner interface { Run(dir, name string, args ...string) }` and package-level function vars (`var syncRunner = executeSync`). A comment forbids widening the interface until a second implementation exists.
Instead of: client libraries, per-verb interfaces, DI containers.
Evidence: `internal/git/git.go:19`, `cmd/sync.go:57`.

**Centralize stderr sniffing behind sentinels.** Some subprocess failures are benign (empty remote, missing branch on remote). Detect them by matching stderr text, but keep that fragile matching in one package that exports sentinels. Callers do `errors.Is(err, git.ErrEmptyRemote)` and report `skipped` instead of a failure.
Instead of: raw error propagation, pre-flight network checks, exit-code-only classification.
Evidence: `internal/git/git.go:118`, `internal/workspace/manager.go:229`.

## Distribution

**Package-manager-aware self-update.** `update` refuses to replace a Homebrew-owned binary and points at `brew upgrade` instead. Downloads verify against the goreleaser `checksums.txt`. The asset-name scheme is a contract documented on both sides (`releaseAssetName()` and `.goreleaser.yaml` reference each other) so neither drifts silently.
Evidence: `cmd/update.go:93`, `.goreleaser.yaml`.

## Testing

**Real-binary harness with recording stubs.** Integration tests (`//go:build integration`, run in Docker) exec the compiled binary in a hermetic sandbox: fresh `$HOME`, PATH-prepended bash shims for `code` and `gh` that record argv or serve canned release responses, bare `file://` git fixtures, pinned env (`NO_COLOR=1`, `TERM=dumb`), and an ETXTBSY retry for parallel exec races (golang/go#22315). Even the VS Code launch and a corrupt-checksum failure path are assertable, with no network.
Instead of: in-process cobra tests with mocks, or tests against real GitHub.
Evidence: `test/integration/harness/harness.go:135`, `test/integration/harness/stubs.go`.

## See also

`tenets/ergo-tenets.md` for the tenets these implement · `kb-agent-practices/decision-hygiene.md` for the decision-comment taxonomy used throughout ergo · `kb-go/idioms/concurrency.md`
