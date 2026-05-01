# ergo — Personal Workspace CLI Specification

> **Language:** Go
> **UI:** Bubble Tea TUI + standard CLI
> **Status:** Specification v1.0
> **Author:** Juan
> **Purpose:** Lightweight workspace management tool that structures context for AI-assisted
> development across VS Code, terminal agents, and local/cloud LLMs.

---

## 1. Overview

### What ergo Does

ergo manages multi-repo development workspaces. It clones repos, organizes them into
a working directory, generates VS Code workspace files, and provides commands to operate
across all repos simultaneously. The TOML configuration file is the single source of truth.

### Core Principles

1. **TOML is the source of truth.** The `.code-workspace` file is a generated output, never edited by hand.
2. **Repos are context.** Pulling repos into a workspace is how you give AI tools deep understanding.
3. **Run everywhere.** `ergo run` executes commands across all (or filtered) roots in the workspace.
4. **Lightweight first.** No databases, no servers, no daemons. Files on disk, git on PATH.
5. **TUI when interactive, CLI when scripted.** Bubble Tea for browsing and selection, plain output for piping.

### Design Boundaries (v1)

**In scope:**
- Workspace TOML config management
- Git clone/pull via shell
- VS Code `.code-workspace` generation
- Cross-repo command execution (`ergo run`)
- Repo tagging, grouping, and filtering
- Non-repo scratch/planning folders
- Status overview (branch, dirty state, behind/ahead)
- Bubble Tea TUI for interactive flows

**Out of scope for v1 (design for later):**
- Doc cache generation (v2 candidate)
- Copilot instructions management (v2 candidate)
- Task routing between local/cloud models (v2 candidate)
- Skills repo integration (v2 candidate)
- Terminal agent integration beyond VS Code (v2 candidate — but architecture shouldn't block it)

---

## 2. Directory Structure

### Global ergo Home

```
~/.ergo/
├── config.toml              # Global config (defaults, GitHub auth, preferences)
├── state/                   # Cached state (last sync times, repo metadata)
│   └── <workspace-name>.json
└── workspaces/              # Workspace definition files
    ├── knowledge-base.toml
    ├── ml-projects.toml
    ├── startup.toml
    └── side-projects.toml
```

### Workspace Root (on disk)

Default location: `~/ergo-workspaces/` (configurable in global config)

```
~/ergo-workspaces/
└── <workspace-name>/
    ├── <workspace-name>.code-workspace   # Generated — do not edit
    ├── repo-a/                           # Cloned repo
    ├── repo-b/                           # Cloned repo
    ├── repo-c/                           # Cloned repo
    ├── scratch/                          # Non-repo folder (defined in TOML)
    └── planning/                         # Non-repo folder (defined in TOML)
```

Everything is flat inside the workspace root. Repos and folders are siblings.
No nesting, no grouping by tag on disk. Tags are a logical concept in the TOML,
not a filesystem concept.

---

## 3. Configuration Format

### Global Config (`~/.ergo/config.toml`)

```toml
[defaults]
workspace_root = "~/ergo-workspaces"    # Where workspace folders are created
default_branch = "main"                  # Default branch to clone/track
github_host = "github.com"              # Default git host

[git]
# Auth is handled by git credential manager on the system
# ergo shells out to git, so whatever auth git has configured works
parallel_clones = 4                      # Max concurrent clones during sync

[tui]
theme = "default"                        # TUI color theme
```

### Workspace Config (`~/.ergo/workspaces/<name>.toml`)

```toml
[workspace]
name = "ml-projects"
description = "Machine learning and deep learning projects"

# VS Code workspace generation settings
[workspace.vscode]
# Settings applied at the workspace level in .code-workspace
[workspace.vscode.settings]
"editor.formatOnSave" = true
"editor.defaultFormatter" = "esbenp.prettier-vscode"

# Repos
[[repos]]
name = "handwriting-recognition"
url = "https://github.com/juan/handwriting-recognition.git"
branch = "main"                          # Optional, defaults to global default
tags = ["ml", "python"]
group = "ml"

# Per-folder VS Code settings override
[repos.vscode_settings]
"python.defaultInterpreterPath" = ".venv/bin/python"

[[repos]]
name = "ergo"
url = "https://github.com/juan/ergo.git"
tags = ["tools", "go"]
group = "tools"

[[repos]]
name = "knowledge-base"
url = "https://github.com/juan/knowledge-base.git"
tags = ["docs", "reference"]
group = "docs"

[[repos]]
name = "bevy-game"
url = "https://github.com/juan/bevy-game.git"
tags = ["rust", "game"]
group = "creative"

# Non-repo folders
[[folders]]
name = "scratch"
purpose = "Temporary working space for AI collaboration and planning"

[[folders]]
name = "planning"
purpose = "Architecture decisions, design docs, and notes"
```

### Field Reference

**`[[repos]]` fields:**

| Field | Required | Type | Description |
|-------|----------|------|-------------|
| `name` | Yes | string | Directory name when cloned. Also used for `--name` filtering. |
| `url` | Yes | string | Git clone URL |
| `branch` | No | string | Branch to checkout. Defaults to `[defaults].default_branch` |
| `tags` | No | string[] | Arbitrary tags for filtering. Multiple tags per repo allowed. |
| `group` | No | string | Logical group. Each repo belongs to at most one group. |
| `vscode_settings` | No | table | VS Code settings overrides for this folder in `.code-workspace` |

**`[[folders]]` fields:**

| Field | Required | Type | Description |
|-------|----------|------|-------------|
| `name` | Yes | string | Directory name, created inside workspace root |
| `purpose` | No | string | Human (and AI) readable description of what this folder is for |
| `vscode_settings` | No | table | VS Code settings overrides for this folder |

---

## 4. Generated `.code-workspace` File

`ergo open` (and `ergo sync`) generates a `.code-workspace` file at the workspace root.

### Example Output

For the TOML config above, ergo generates:

```json
{
  "ergo-workspace": "ml-projects",
  "folders": [
    {
      "name": "handwriting-recognition",
      "path": "handwriting-recognition"
    },
    {
      "name": "ergo",
      "path": "ergo"
    },
    {
      "name": "knowledge-base",
      "path": "knowledge-base"
    },
    {
      "name": "bevy-game",
      "path": "bevy-game"
    },
    {
      "name": "scratch",
      "path": "scratch"
    },
    {
      "name": "planning",
      "path": "planning"
    }
  ],
  "settings": {
    "editor.formatOnSave": true,
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "folders[0].settings": {
    "python.defaultInterpreterPath": ".venv/bin/python"
  }
}
```

> **Note:** The `"ergo-workspace"` key at the top level is a custom field that VS Code
> ignores but allows ergo (and any other tooling) to identify which workspace config
> generated this file. This is essential for the terminal agent integration path.

### Generation Rules

1. Repos appear in the order defined in the TOML
2. Folders appear after all repos
3. Workspace-level `[workspace.vscode.settings]` maps to top-level `"settings"`
4. Per-repo/folder `vscode_settings` maps to folder-level settings
5. The file is regenerated on every `ergo sync` and `ergo open`
6. If the file already exists, it is overwritten (TOML is source of truth)

---

## 5. Commands

### `ergo init`

Create a new workspace.

```
ergo init <workspace-name>
```

**Behavior:**
1. Creates `~/.ergo/workspaces/<workspace-name>.toml` with skeleton config
2. Creates `~/ergo-workspaces/<workspace-name>/` directory
3. Opens the TOML in `$EDITOR` for the user to populate (or launches TUI to add repos interactively)

**TUI flow (if no args or `--interactive`):**
1. Prompt for workspace name and description
2. Prompt to add repos (URL, name, tags, group)
3. Prompt to add folders
4. Write TOML, create directory

**Flags:**
- `--interactive` / `-i`: Force TUI flow even if name is provided

---

### `ergo sync`

Clone missing repos, pull existing ones, create missing folders, regenerate `.code-workspace`.

```
ergo sync [workspace-name]
```

**Behavior:**
1. Read workspace TOML
2. For each `[[repos]]`:
   - If directory doesn't exist: `git clone <url> --branch <branch> <name>`
   - If directory exists: `git fetch && git pull` (on the configured branch)
3. For each `[[folders]]`: create directory if missing
4. Regenerate `.code-workspace` file
5. Display summary (cloned N, pulled N, created N folders, N errors)

**If `workspace-name` is omitted:**
- If CWD is inside a workspace root, sync that workspace
- Otherwise, launch TUI to select a workspace

**Flags:**
- `--clone-only`: Skip pull on existing repos (faster for initial setup)
- `--dry-run`: Show what would happen without doing it
- `--parallel` / `-p`: Clone/pull repos in parallel (default: sequential)
- Filtering flags apply (see §5.7)

---

### `ergo open`

Generate `.code-workspace` file and open VS Code.

```
ergo open [workspace-name]
```

**Behavior:**
1. Regenerate `.code-workspace` from TOML (always, to pick up config changes)
2. Run `code <workspace-name>.code-workspace`

**If workspace hasn't been synced yet:** run `ergo sync` first, then open.

**If `workspace-name` is omitted:**
- If CWD is inside a workspace root, open that workspace
- Otherwise, launch TUI to select a workspace

---

### `ergo status`

Show the state of all repos in a workspace.

```
ergo status [workspace-name]
```

**Output (TUI table):**

```
┌─────────────────────────┬──────────┬────────┬─────────┬──────────┐
│ Repo                    │ Branch   │ Status │ Behind  │ Group    │
├─────────────────────────┼──────────┼────────┼─────────┼──────────┤
│ handwriting-recognition │ main     │ clean  │ —       │ ml       │
│ ergo                    │ feat/tui │ dirty  │ 3       │ tools    │
│ knowledge-base          │ main     │ clean  │ 1       │ docs     │
│ bevy-game               │ main     │ dirty  │ —       │ creative │
└─────────────────────────┴──────────┴────────┴─────────┴──────────┘
```

**Status values:**
- `clean` — no uncommitted changes
- `dirty` — uncommitted changes present
- `uncloned` — defined in TOML but not yet on disk

**Flags:**
- `--short` / `-s`: One-line-per-repo, no table borders (for scripting)
- Filtering flags apply (see §5.7)

---

### `ergo add`

Add a repo or folder to the workspace TOML interactively.

```
ergo add [workspace-name]
```

**TUI flow:**
1. Select: repo or folder?
2. If repo: prompt for URL, name (auto-derived from URL), branch, tags, group
3. If folder: prompt for name, purpose
4. Write updated TOML
5. Optionally run `ergo sync` to clone immediately

**Shorthand (non-interactive):**
```
ergo add repo https://github.com/juan/new-project.git --tags=rust,game --group=creative
ergo add folder design-docs --purpose="Design exploration space"
```

---

### `ergo remove`

Remove a repo or folder from the workspace TOML.

```
ergo remove [workspace-name]
```

**TUI flow:**
1. Show list of repos and folders
2. Select items to remove (multi-select)
3. Confirm
4. Remove from TOML
5. Ask: also delete from disk? (default: no)

**Shorthand:**
```
ergo remove repo old-experiment
ergo remove folder scratch --delete   # Also removes from disk
```

---

### `ergo run`

Execute a command across all (or filtered) roots in the workspace.

```
ergo run [workspace-name] -- <command>
```

**Behavior:**
1. For each repo (and optionally folder) in the workspace:
   - `cd` into the directory
   - Execute `<command>` via shell
   - Capture stdout/stderr
2. Display results sequentially (one repo at a time, with headers)

**Output format:**
```
━━━ handwriting-recognition ━━━
On branch main
nothing to commit, working tree clean

━━━ ergo ━━━
On branch feat/tui
Changes not staged for commit:
  modified:   cmd/root.go

━━━ knowledge-base ━━━
On branch main
nothing to commit, working tree clean
```

**Flags:**
- `--parallel` / `-p`: Run in parallel (default: sequential). In parallel mode, output is buffered per-repo and printed atomically when each finishes.
- `--fail-fast`: Stop on first non-zero exit code
- `--include-folders`: Also run in non-repo `[[folders]]` (default: repos only)
- Filtering flags apply (see §5.7)

**Examples:**
```bash
# Git status across all repos
ergo run -- git status

# Pull all repos
ergo run -- git pull

# Run tests only in Go repos
ergo run --tags=go -- go test ./...

# Check for uncommitted changes in the ml group
ergo run --group=ml -- git diff --stat

# Run a command in a specific repo
ergo run --name=ergo -- go build ./...
```

---

### `ergo list`

List all configured workspaces.

```
ergo list
```

**Output:**
```
┌───────────────────┬──────────────────────────────────────────┬───────┐
│ Workspace         │ Description                              │ Repos │
├───────────────────┼──────────────────────────────────────────┼───────┤
│ ml-projects       │ Machine learning and deep learning       │ 4     │
│ startup           │ AI-native knowledge graph                │ 6     │
│ side-projects     │ 30 Before 30 project repos               │ 8     │
│ knowledge-base    │ Architectural knowledge base              │ 2     │
└───────────────────┴──────────────────────────────────────────┴───────┘
```

---

### 5.7 Filtering (Global Flags)

Filtering flags are available on any command that operates across repos:
`sync`, `status`, `run`, `open` (to limit which folders appear).

```
--name=<pattern>     # Filter by repo name. Supports glob: --name=*-service
--group=<group>      # Filter by group
--group              # (no value) Launch TUI to select group(s)
--tags=<t1,t2>       # Filter by tags (repos matching ANY of the listed tags)
```

**Combining filters:** Filters are AND-ed. `--group=ml --tags=python` means
repos that are in group `ml` AND have the tag `python`.

---

## 6. Architecture

### Package Structure

```
ergo/
├── cmd/                        # Cobra command definitions
│   ├── root.go                 # Root command, global flags
│   ├── init.go
│   ├── sync.go
│   ├── open.go
│   ├── status.go
│   ├── add.go
│   ├── remove.go
│   ├── run.go
│   └── list.go
├── internal/
│   ├── config/                 # TOML parsing and validation
│   │   ├── global.go           # Global config (~/.ergo/config.toml)
│   │   ├── workspace.go        # Workspace config parsing
│   │   └── types.go            # Config structs
│   ├── workspace/              # Workspace operations
│   │   ├── manager.go          # Clone, pull, sync logic
│   │   ├── filter.go           # Tag/group/name filtering
│   │   ├── runner.go           # Cross-repo command execution
│   │   └── status.go           # Repo state inspection
│   ├── vscode/                 # VS Code integration
│   │   └── generate.go         # .code-workspace file generation
│   ├── git/                    # Git operations (shells out)
│   │   └── git.go              # Clone, pull, status, fetch
│   └── tui/                    # Bubble Tea components
│       ├── app.go              # Main TUI application model
│       ├── workspace_select.go # Workspace selection list
│       ├── repo_table.go       # Status table view
│       ├── add_form.go         # Add repo/folder form
│       ├── remove_select.go    # Multi-select removal
│       ├── run_output.go       # Command output viewer
│       └── styles.go           # Lipgloss styles
├── go.mod
├── go.sum
├── main.go
└── README.md
```

### Key Dependencies

| Package | Purpose |
|---------|---------|
| `github.com/spf13/cobra` | CLI command framework |
| `github.com/charmbracelet/bubbletea` | TUI framework |
| `github.com/charmbracelet/lipgloss` | TUI styling |
| `github.com/charmbracelet/bubbles` | TUI components (tables, text inputs, spinners) |
| `github.com/BurntSushi/toml` | TOML parsing |
| `github.com/gobwas/glob` | Glob pattern matching for `--name` |

### Git Operations

All git operations shell out to `git` on PATH. No go-git dependency.

```go
// internal/git/git.go

func Clone(url, dest, branch string) error {
    args := []string{"clone", url, dest}
    if branch != "" {
        args = append(args, "--branch", branch)
    }
    return exec.Command("git", args...).Run()
}

func Pull(repoPath string) error {
    cmd := exec.Command("git", "pull")
    cmd.Dir = repoPath
    return cmd.Run()
}

func Status(repoPath string) (RepoState, error) {
    // git status --porcelain for dirty check
    // git rev-list --count HEAD..@{u} for behind count
}
```

---

## 7. State Management

### Cached State (`~/.ergo/state/<workspace>.json`)

Lightweight cache to avoid redundant git operations:

```json
{
  "workspace": "ml-projects",
  "last_sync": "2026-04-30T10:30:00Z",
  "repos": {
    "handwriting-recognition": {
      "last_pulled": "2026-04-30T10:30:00Z",
      "commit_hash": "abc123f"
    }
  }
}
```

This is a performance optimization, not a source of truth. If the cache is
missing or corrupt, ergo just does a full sync. No user-facing commands
interact with this file directly.

---

## 8. Error Handling

| Scenario | Behavior |
|----------|----------|
| Git not on PATH | Error on startup: "git is required. Install git and try again." |
| Clone fails (auth, network) | Log error, continue with other repos, report at end |
| TOML parse error | Error with line number and context |
| Workspace dir already exists (init) | Ask to adopt or abort |
| Repo dir exists but isn't the expected repo | Warn, skip, report |
| VS Code not installed (`code` not on PATH) | Generate `.code-workspace` but warn that `code` wasn't found |
| No workspace name and CWD isn't in a workspace | Launch TUI selector |

---

## 9. Future Extensions (v2+ Design Considerations)

These are explicitly out of v1 scope but the architecture should not block them.

### 9.1 Doc Cache Generation

A command like `ergo docs generate` that runs a documentation extraction pass
over repos and stores the output in a well-known location. This is the
"documentation is a cache" insight from the AI SDLC.

**Why the architecture supports it:** Each `[[repos]]` entry could gain a
`[repos.docs]` table with doc generation config. The generated docs land in
a folder within the workspace root. The `.code-workspace` file could optionally
include the docs folder so Copilot can read them.

### 9.2 Copilot Instructions Management

A `[workspace.copilot]` section in the TOML that generates
`.github/copilot-instructions.md` or VS Code workspace-level settings
for Copilot behavior.

### 9.3 Terminal Agent Integration

`ergo agent` that launches a terminal agent (Claude Code, aider, etc.)
with the workspace context pre-loaded. The agent would receive:
- List of repos and their purposes
- Workspace-level instructions
- Current state (branches, dirty files)

The `"ergo-workspace"` field in `.code-workspace` makes it possible for
any tool to discover which workspace it's operating in and read the TOML.

### 9.4 Task Routing

Configuration in the TOML for routing tasks to different models:
```toml
[workspace.routing]
local_model = "qwen2.5-coder:32b"
cloud_model = "sonnet-4.6"

[workspace.routing.rules]
autocomplete = "local"
refactor = "local"
architecture = "cloud"
code_review = "cloud"
```

### 9.5 Skills Integration

A `[[skills]]` section that pulls in skills repos — repos that contain
instructions, templates, and patterns that AI tools can reference:
```toml
[[skills]]
name = "go-patterns"
url = "https://github.com/juan/go-patterns.git"
```

Skills repos would be cloned into the workspace alongside regular repos
but tagged differently, potentially excluded from `ergo run` by default.

---

## 10. Build & Distribution

### Build

```bash
go build -o ergo .
```

### Install

```bash
go install github.com/juan/ergo@latest
```

### Homebrew (stretch goal)

```bash
brew install juan/tap/ergo
```

### Release

Use GoReleaser for cross-platform builds. Primary target: macOS arm64 (Apple Silicon).
Secondary: macOS amd64, Linux amd64/arm64.

---

## 11. Example Session

```bash
# Create a new workspace
$ ergo init ml-projects
Created workspace: ml-projects
Config: ~/.ergo/workspaces/ml-projects.toml
Root:   ~/ergo-workspaces/ml-projects/

# Edit the TOML to add repos (or use ergo add)
$ ergo add repo https://github.com/juan/handwriting-recognition.git \
    --tags=ml,python --group=ml

$ ergo add repo https://github.com/juan/ergo.git \
    --tags=tools,go --group=tools

$ ergo add folder scratch --purpose="Temporary AI collaboration space"

# Sync: clone repos, create folders, generate .code-workspace
$ ergo sync
Cloning handwriting-recognition... done
Cloning ergo... done
Creating scratch/... done
Generated ml-projects.code-workspace
Sync complete: 2 cloned, 0 pulled, 1 folder created

# Open in VS Code
$ ergo open
Opening ml-projects.code-workspace in VS Code...

# Check status across all repos
$ ergo status
┌─────────────────────────┬────────┬────────┬────────┬───────┐
│ Repo                    │ Branch │ Status │ Behind │ Group │
├─────────────────────────┼────────┼────────┼────────┼───────┤
│ handwriting-recognition │ main   │ clean  │ —      │ ml    │
│ ergo                    │ main   │ dirty  │ 2      │ tools │
└─────────────────────────┴────────┴────────┴────────┴───────┘

# Run tests only in Go repos
$ ergo run --tags=go -- go test ./...
━━━ ergo ━━━
ok  	github.com/juan/ergo/internal/config	0.003s
ok  	github.com/juan/ergo/internal/workspace	0.012s

# Pull all repos
$ ergo run -- git pull
━━━ handwriting-recognition ━━━
Already up to date.

━━━ ergo ━━━
Updating abc123f..def456a
 2 files changed, 45 insertions(+), 12 deletions(-)
```