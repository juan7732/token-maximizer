# ergo — Usage Guide

`ergo` manages multi-repo development workspaces. It clones repos, organizes
them into a working directory, generates VS Code `.code-workspace` files, and
provides commands to operate across all repos simultaneously.

The TOML config under `~/.ergo/workspaces/<name>.toml` is the single source of
truth. The `.code-workspace` file is a derived artifact.

---

## Quick start

```sh
ergo init my-workspace         # define a workspace interactively
ergo open my-workspace         # clone repos, generate workspace, launch VS Code
ergo status                    # see git state across all repos
ergo run -- git pull           # run a command in every repo
```

Most commands accept `[workspace-name]` as a positional argument. If omitted,
ergo detects the workspace from the current working directory.

---

## Global flags

| Flag            | Description          |
| --------------- | -------------------- |
| `-h, --help`    | Help for any command |
| `-v, --version` | Print version        |
| `--no-color`    | Disable color output |

---

## Commands

### `ergo init [workspace-name]`

Create a new workspace definition via a guided TUI.

Writes `~/.ergo/workspaces/<workspace-name>.toml` with the repos and folders
you configure. Does **not** create directories or clone anything — run
`ergo open` to materialize the workspace.

---

### `ergo open [workspace-name]`

Open a workspace in VS Code, creating it on disk if it doesn't exist yet.

- **First-time use:** clones all repos, creates folders, generates the
  `.code-workspace` file, then launches VS Code.
- **Subsequent use:** regenerates `.code-workspace` only if its content has
  changed (smart regeneration), then launches VS Code. Does **not** re-clone
  or pull — use `ergo sync` for that.

**Flags**

| Flag          | Description                                                           |
| ------------- | --------------------------------------------------------------------- |
| `--print-dir` | Print the workspace directory to stdout instead of launching VS Code. |

**Shell wrapper for `cd`-ing into the workspace** (a child process can't
change the parent shell's cwd):

```sh
ergocd() { cd "$(ergo open --print-dir "$@")"; }
```

---

### `ergo list`

List all workspaces defined in `~/.ergo/workspaces/` with their sync status.

---

### `ergo status [workspace-name]`

Show branch, dirty state, and ahead/behind count for each repo.

When run inside a standalone git repo (outside any ergo workspace), shows
status for just that one repo.

**Flags**

| Flag             | Description                                                      |
| ---------------- | ---------------------------------------------------------------- |
| `-s, --short`    | One-line-per-repo output, no table borders (scripting-friendly). |
| `--group <name>` | Filter repos to this group.                                      |
| `--name <glob>`  | Filter repos by name (glob pattern, case-insensitive).           |
| `--tags <list>`  | Filter repos by tags, any-match (comma-separated or repeated).   |

---

### `ergo sync [workspace-name]`

Synchronize the workspace directory on disk with the TOML configuration.

For each **repo**:
- If the directory doesn't exist → clone it.
- If it exists and `auto_pull = true` → `git pull`.
- If it exists and `auto_pull = false` → skip.

For each **folder**:
- If the directory doesn't exist → create it.
- If `git = true` and not yet a git repo → `git init`.

Sync **never deletes** by default.

**Flags**

| Flag             | Description                                                                            |
| ---------------- | -------------------------------------------------------------------------------------- |
| `--add`          | Add repos/folders found on disk but not in TOML to the config (requires confirmation). |
| `--force`        | Delete directories that are not in the TOML (requires confirmation).                   |
| `--group <name>` | Filter repos to this group.                                                            |
| `--name <glob>`  | Filter repos by name (glob, case-insensitive).                                         |
| `--tags <list>`  | Filter repos by tags, any-match.                                                       |

---

### `ergo run [workspace-name] -- <command> [args...]`

Execute a command across all (or filtered) repos in the workspace. The
command **must** be preceded by `--` to separate it from ergo flags.

```sh
ergo run -- git status
ergo run --tags=go -- go test ./...
ergo run ml-projects -- git pull
```

**Group exclusion:** repos in groups listed in `[run].excluded_groups`
(global config) are automatically skipped. Override with `--include-group`
or `--all`.

When run inside a standalone git repo (outside any ergo workspace), the
command runs in that repo only — filter flags and group exclusion are ignored.

**Flags**

| Flag                     | Description                                      |
| ------------------------ | ------------------------------------------------ |
| `--all`                  | Include all repos, ignoring `excluded_groups`.   |
| `--include-group <name>` | Override group exclusion for one specific group. |
| `--include-folders`      | Also run in non-repo `[[folders]]`.              |
| `--fail-fast`            | Stop after the first non-zero exit code.         |
| `--group <name>`         | Filter repos to this group.                      |
| `--name <glob>`          | Filter repos by name (glob, case-insensitive).   |
| `--tags <list>`          | Filter repos by tags, any-match.                 |

---

### `ergo show [group | all]`

Filter the VS Code workspace view by regenerating `.code-workspace` to include
only repos matching the filter. The filter is recorded in the ergo metadata
object so it persists between runs.

```sh
ergo show ml           # filter to the "ml" group
ergo show --tag=go     # filter to repos tagged "go"
ergo show all          # clear filter, restore full view
ergo show              # interactive group/tag selector
```

Modifies `.code-workspace` only — never the TOML or filesystem. The workspace
must be materialized on disk first (`ergo open`). Operates on the workspace
detected from the current working directory.

**Flags**

| Flag           | Description                                          |
| -------------- | ---------------------------------------------------- |
| `--tag <list>` | Filter repos by tag (repeatable or comma-separated). |

---

### `ergo add [workspace-name]`

Add a repo or folder to a workspace definition.

Without subcommands, launches an interactive TUI. Use `repo` or `folder`
subcommands for non-interactive shorthand.

#### `ergo add repo <url>`

```sh
ergo add repo https://github.com/owner/repo.git
ergo add repo https://github.com/owner/repo.git --tags=go,tools --group=tools
ergo add repo https://github.com/owner/utils.git --name=utils-personal
```

| Flag             | Description                                  |
| ---------------- | -------------------------------------------- |
| `--name <name>`  | Explicit repo name (overrides derived name). |
| `--group <name>` | Group name.                                  |
| `--tags <list>`  | Comma-separated tags.                        |

#### `ergo add folder <name>`

```sh
ergo add folder scratch
ergo add folder planning --git
```

| Flag    | Description                              |
| ------- | ---------------------------------------- |
| `--git` | Run `git init` when creating the folder. |

---

### `ergo remove [workspace-name]`

Remove one or more repos or folders from a workspace definition.

Without subcommands, launches a multi-select TUI. Use `repo` or `folder`
subcommands for non-interactive shorthand.

By default, only the TOML entry is removed — files on disk are untouched.
Use `--force` to also delete the directory from disk (requires confirmation).

```sh
ergo remove repo <name>            # remove from TOML only
ergo remove folder <name> --force  # also delete from disk
```

| Flag      | Description                                                  |
| --------- | ------------------------------------------------------------ |
| `--force` | Also delete the directory from disk (requires confirmation). |

---

### `ergo edit [workspace-name]`

Open the workspace TOML configuration file in VS Code for editing. Changes
take effect the next time `ergo sync` or `ergo open` runs.

| Flag           | Description                                                  |
| -------------- | ------------------------------------------------------------ |
| `-g, --global` | Edit the global ergo config (`~/.ergo/config.toml`) instead. |

---

### `ergo validate [workspace-name]`

Validate the TOML configuration for a workspace. Checks for required fields,
name collisions, and other spec constraints. Exits non-zero if issues are
found.

| Flag    | Description              |
| ------- | ------------------------ |
| `--all` | Validate all workspaces. |

---

### `ergo update`

Check GitHub releases for a newer version of ergo and replace the running
binary atomically.

---

### `ergo completion <shell>`

Generate the autocompletion script for the specified shell (bash, zsh, fish,
powershell). Run `ergo completion --help` for shell-specific install
instructions.

---

## Common filter flags

Several commands (`status`, `sync`, `run`) share the same filtering vocabulary:

| Flag             | Behavior                                        |
| ---------------- | ----------------------------------------------- |
| `--group <name>` | Only include repos in this group.               |
| `--name <glob>`  | Glob match against repo name, case-insensitive. |
| `--tags <list>`  | Any-match against repo tags.                    |

---

## Typical workflows

**Bootstrap a new machine**

```sh
ergo list                      # see existing workspace definitions
ergo open my-workspace         # clone everything and launch VS Code
```

**Daily refresh**

```sh
ergo sync                      # pull repos with auto_pull = true
ergo status -s                 # quick overview of what's dirty
```

**Focused work on a subset**

```sh
ergo show ml                   # narrow VS Code view to the "ml" group
ergo run --group=ml -- pytest  # run tests just in that group
ergo show all                  # restore full view
```

**Add a new repo to an existing workspace**

```sh
ergo add repo https://github.com/owner/new.git --group=tools --tags=go
ergo sync                      # clone it
```
