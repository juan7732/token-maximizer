# Shell & CLI

**Category:** Ops · Automation

**Task runner:** Just over Make for new work (no tab traps, args, clearer). Make if ubiquity needed. One entrypoint: `build test run fmt lint`.

**Bash safety:**
```sh
set -euo pipefail
IFS=$'\n\t'
```
Quote vars, prefer `$()`, check with shellcheck.

**Personal CLI toolkit:** fzf (fuzzy), ripgrep (search), fd (find), jq/yq (data), bat, zoxide, gh, direnv (per-dir env).

**Aliases that earn it:** git shortcuts, project jump (zoxide), `serve`, log tailers. Keep dotfiles versioned.

**See also:** justfile-starter (kb-templates), editor-productivity.md.
