# Editor Productivity

**Category:** Ops · Automation

**LSP:** one server per language (gopls, rust-analyzer, tsserver, pyright). Format-on-save, organize imports, inline diagnostics. Learn: go-to-def, rename-symbol, find-refs, code actions.

**Snippets:** keep a personal library; trigger common scaffolds (test, handler, struct). Editor-native or via `kb-templates`.

**Linters/format:** golangci-lint, clippy, eslint+prettier/biome, ruff. Config committed; same in CI.

**Git hooks** (pre-commit/lefthook):
```yaml
pre-commit: [fmt, lint, typecheck]
pre-push: [test]
```
Fast hooks only; heavy checks in CI. Never `--no-verify` to skip.

**See also:** code-generation.md, kb-ops/checklists/code-review.md.
