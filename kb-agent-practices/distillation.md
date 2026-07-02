# Distillation

**Category:** Agent Practices · **Captured:** 2026-07-01, from distilling ergo into `kb-opinionated-cli/patterns-from-ergo.md`

Extract the differentiated knowledge from a codebase you trust into a kb file. The selection of the source codebase is where taste enters; the process below is mechanical. The executable form is the `distill` skill in juan-skills; if they drift, the skill wins.

Rule from the first calibration pass: only distill codebases the owner has named as verified-good. Recent activity is not vetting.

## The filter

A pattern earns capture only if all three hold:

1. **Opinionated:** a choice among plausible alternatives, not forced by language or ecosystem.
2. **Non-obvious:** a strong model would not reliably produce it when asked to build the same thing. Generic idioms are excluded.
3. **Transferable:** useful in the next project, not source-repo trivia.

Require the extraction to also return a rejected-as-generic list with one clause each. It calibrates the filter and shows what was considered.

## The process

1. **Extract.** One agent deep-reads the codebase with the three-part filter. Per pattern: claim (one sentence), why, evidence (file:line plus minimal excerpt), the rejected alternative, when to reuse.
2. **Verify.** Read the load-bearing files yourself, or in a second pass, and spot-check every claim you did not personally see. Extraction agents are persuasive; treat their output as candidates.
3. **Curate.** Apply the filter again with your own judgment. Merge overlapping patterns, cut survivors that only technically pass.
4. **Human review.** The owner's pass is where personal taste gets injected. What they cut recalibrates the filter for next time.

## Evidence rules

Pin the source commit in the file header. File paths and line numbers rot; a pinned hash keeps them traceable. If the source repo will not be present in consuming workspaces and cannot be pinned, drop the evidence rather than misguide a future agent.

## Observed cost

ergo (8.3k lines of Go): one extraction agent, about 94k tokens, 4 minutes. Cheap enough to point at any codebase worth learning from.
