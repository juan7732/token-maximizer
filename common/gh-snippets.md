# gh CLI Snippets Reference

> **Purpose:** Battle-tested gh CLI patterns for personal project management.
> **Context:** GitHub.com (not Enterprise), macOS, repos under a single personal account.

---

## Authentication & Setup

```bash
# Login (browser-based OAuth — do this once)
gh auth login

# Verify auth status
gh auth status

# Set default editor
gh config set editor "code --wait"

# Set default git protocol (ssh recommended for personal)
gh config set git_protocol ssh

# Check current defaults
gh config list
```

---

## Repo Creation & Management

### Create Repos

```bash
# Create public repo with sensible defaults
gh repo create my-project --public --clone \
  --description "Short description" \
  --license mit

# Create private repo
gh repo create my-project --private --clone

# Create from current directory (already has code)
cd existing-project
git init
gh repo create --public --source=. --push

# Create repo without cloning (useful for ergo workflows —
# create on GitHub, let ergo sync handle the clone)
gh repo create kb-core --public --description "Cross-cutting patterns and architecture"

# Batch create repos (knowledge base setup)
for repo in kb-core kb-go kb-rust kb-typescript kb-python kb-templates kb-ops kb-comparisons; do
  gh repo create "$repo" --public --description "Knowledge base: $repo"
  echo "Created $repo"
done
```

### Repo Settings

```bash
# Update description
gh repo edit --description "Updated description"

# Change visibility
gh repo edit --visibility public
gh repo edit --visibility private

# Enable/disable features
gh repo edit --enable-wiki=false --enable-issues=true

# Set default branch
gh repo edit --default-branch main

# Add topics (useful for discovery)
gh repo edit --add-topic go,cli,workspace-management

# Delete repo (careful — no undo)
gh repo delete owner/repo --yes
```

### Repo Info

```bash
# View repo details
gh repo view

# View specific repo
gh repo view juan/ergo

# List your repos (useful for auditing)
gh repo list --limit 50

# List repos with specific topic
gh repo list --topic knowledge-base

# JSON output for scripting
gh repo list --json name,description,isPrivate --limit 100
```

---

## Branch Management

```bash
# View current repo's default branch
gh repo view --json defaultBranchRef --jq '.defaultBranchRef.name'

# Set branch protection (main branch)
gh api repos/{owner}/{repo}/branches/main/protection \
  --method PUT \
  --field required_status_checks='null' \
  --field enforce_admins=false \
  --field required_pull_request_reviews='null' \
  --field restrictions='null'

# Remove branch protection
gh api repos/{owner}/{repo}/branches/main/protection --method DELETE
```

---

## Issues & Project Tracking

```bash
# Create issue
gh issue create --title "Add rate limiter pattern" --body "..." --label enhancement

# Create issue with template (reads from stdin or editor)
gh issue create --title "Bug: sync fails on empty repo" --label bug

# List open issues
gh issue list

# List issues by label
gh issue list --label "good first issue"

# Close issue
gh issue close 42 --reason completed

# Batch create issues (project planning)
cat <<'EOF' | while IFS='|' read -r title label; do
  gh issue create --title "$title" --label "$label"
done
Add error handling patterns|content
Add concurrency patterns|content
Add API design guide|content
Add testing strategy|content
Write Go idioms|content
Write Rust idioms|content
EOF

# View issue
gh issue view 42

# Search issues across repos
gh search issues "error handling" --owner=juan --limit=10
```

---

## Pull Requests

```bash
# Create PR from current branch
gh pr create --title "Add worker pool pattern" --body "Covers Go and Rust implementations"

# Create PR and auto-fill from commits
gh pr create --fill

# Create draft PR
gh pr create --draft --title "WIP: task routing"

# List open PRs
gh pr list

# View PR details
gh pr view 7

# Merge PR (squash is cleanest for personal repos)
gh pr merge 7 --squash --delete-branch

# Check PR status for current branch
gh pr status
```

---

## Releases & Tags

```bash
# Create release from tag
gh release create v0.1.0 --title "ergo v0.1.0" --notes "Initial release"

# Create release with auto-generated notes (from PR titles since last release)
gh release create v0.2.0 --generate-notes

# Create release with binary attachments (after goreleaser)
gh release create v0.1.0 ./dist/*.tar.gz ./dist/*.zip --title "ergo v0.1.0"

# List releases
gh release list

# Download release assets
gh release download v0.1.0 --pattern "*.tar.gz"

# Delete release
gh release delete v0.1.0 --yes
```

---

## GitHub Actions

```bash
# List workflows
gh workflow list

# View recent runs
gh run list --limit 10

# View specific run
gh run view 12345

# Watch a run in real-time
gh run watch 12345

# Trigger workflow manually (workflow_dispatch)
gh workflow run ci.yml --ref main

# Download artifacts from a run
gh run download 12345

# View run logs
gh run view 12345 --log

# Re-run failed jobs
gh run rerun 12345 --failed
```

---

## GitHub API (Direct Access)

For operations gh doesn't have built-in commands for:

```bash
# Generic API call (auto-handles auth and base URL)
gh api repos/{owner}/{repo}

# POST with JSON body
gh api repos/{owner}/{repo}/labels \
  --method POST \
  --field name="kb-content" \
  --field color="0e8a16" \
  --field description="Knowledge base content"

# Paginated listing with jq
gh api repos/{owner}/{repo}/commits --paginate --jq '.[].sha'

# GraphQL query
gh api graphql -f query='
  query {
    viewer {
      repositories(first: 10, orderBy: {field: UPDATED_AT, direction: DESC}) {
        nodes { name updatedAt }
      }
    }
  }
'
```

---

## Gists (Quick Sharing)

```bash
# Create public gist from file
gh gist create snippet.go --public --desc "Worker pool pattern in Go"

# Create from stdin
echo "quick note" | gh gist create --filename notes.md

# Create multi-file gist
gh gist create file1.go file2.go --desc "Related patterns"

# List your gists
gh gist list --limit 20

# Edit gist
gh gist edit <gist-id>
```

---

## Cloudflare Pages Deployment Patterns

Since you use Cloudflare for frontend hosting, these patterns pair with gh:

```bash
# In GitHub Actions, deploy to Cloudflare Pages after build
# .github/workflows/deploy.yml
# (use wrangler CLI in the action, triggered by gh push)

# Locally: deploy preview from branch
npx wrangler pages deploy ./dist --project-name=my-app --branch=$(git branch --show-current)

# Locally: deploy production
npx wrangler pages deploy ./dist --project-name=my-app --branch=main
```

---

## Scripting Patterns

### JSON Output + jq

```bash
# Get all repo names as plain text
gh repo list --json name --jq '.[].name'

# Get repos with their clone URLs
gh repo list --json name,sshUrl --jq '.[] | "\(.name)\t\(.sshUrl)"'

# Find repos without descriptions
gh repo list --json name,description \
  --jq '.[] | select(.description == "") | .name'

# Count issues by label
gh issue list --json labels --jq '[.[].labels[].name] | group_by(.) | map({(.[0]): length}) | add'
```

### Cross-Repo Operations (ergo run alternative for pure gh tasks)

```bash
# Check CI status across multiple repos
for repo in kb-core kb-go kb-rust ergo; do
  echo "=== $repo ==="
  gh run list --repo "juan/$repo" --limit 1 --json status,conclusion \
    --jq '.[0] | "\(.status) - \(.conclusion)"'
done

# Sync labels across all kb repos
LABEL_NAME="content"
LABEL_COLOR="0e8a16"
for repo in kb-core kb-go kb-rust kb-typescript kb-python kb-templates kb-ops kb-comparisons; do
  gh api "repos/juan/$repo/labels" \
    --method POST \
    --field name="$LABEL_NAME" \
    --field color="$LABEL_COLOR" 2>/dev/null || echo "$repo: label exists"
done

# Bulk update repo topics
for repo in kb-core kb-go kb-rust kb-typescript kb-python; do
  gh repo edit "juan/$repo" --add-topic knowledge-base,patterns
done
```

---

## Ergo Integration Candidates

Operations that make sense to absorb into ergo eventually:

```bash
# ergo add repo --create
# Under the hood:
gh repo create "$NAME" --public --description "$DESC"
# Then adds to TOML and runs sync

# ergo init --create-repos
# Under the hood: creates all [[repos]] on GitHub that don't exist yet

# ergo status --remote
# Under the hood: for each repo, also checks gh run status
# Shows: branch | dirty | behind | CI status

# ergo run --group=knowledge -- gh issue list
# Already works with ergo run — no wrapping needed
```