# Lock-In: Inverted Orchestration

**Category:** Agent Practices · **Source:** `github.com/juan7732/juan-skills` @ `3fde96d` (`.claude/skills/lock-in/SKILL.md`) · **Captured:** 2026-07-02

The executable form is the `lock-in` skill; this doc records the pattern and why it works. Do not duplicate the skill's mechanics here; if they drift, the skill wins.

## The pattern

Invert the default working model. Instead of the human orchestrating one agent session, the agent runs a master session as orchestrator/PM: it decomposes work, decides next steps, and delegates implementation via self-contained copy-paste prompts. The human is lead plus execution multiplexer, running those prompts across modalities the agent cannot reach (other Claude Code sessions, Deep Research, Artifacts, human-only calls like spend and legal). Mutual delegation: each side hands work to the other.

Wrapped around it is a time-boxed prove-or-kill sprint: one thesis, a hard window, prove/kill criteria written before any evidence exists.

## Why it works

- **The master session's context is spent on judgment, not production.** Deliverables consume context fast; a session that only decomposes, reviews, and decides stays coherent for the whole sprint.
- **Durable state on disk survives context loss.** Charter, sprint board, and append-only session log are the source of truth; any fresh session re-anchors from them in one read. Same principle as decision hygiene: intent must outlive the session that produced it.
- **Gates before evidence kills drift.** Criteria written in advance cannot be re-graded after results arrive. The skill also names the failure modes: confidence inflation as claims move between documents, and closed citation loops that only a fresh-eyes review breaks.
- **It contains documentation churn.** One board, one log, one charter. Sub-sessions report back into them instead of spawning parallel docs.

## Observed result

FloorplanIQ sprint, July 2026: roughly a month of planned work landed in about two days, and documentation/confusion churn in the large workspace dropped enough to re-focus the project.

## When to use

Starting or resuming a focused sprint, or when a workspace shows drift symptoms: multiplying documents, re-litigated decisions, no clear gate. Not a general working mode; it needs a window, a thesis, and an end decision.

## Distribution note

Skills in a repo's `.claude/skills/` are discovered automatically when the repo is an ergo workspace member, so `ergo add repo juan-skills` installs the practice into any workspace. Project-level skills override on name collision, letting a project carry a specialized variant. This makes a skills repo the third kb content type: reference (kb-*), opinions (kb-opinionated-*), and executable practices (juan-skills).

## See also

`decision-hygiene.md` · `distillation.md` · `juan-skills/.claude/skills/lock-in/SKILL.md`
