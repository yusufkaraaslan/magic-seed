---
name: pr-flow
description: Use when the developer says "validate X for PR", "PR check", "ready for PR", "pre-PR review", "open PR for X", or "is X ready to merge". Pre-PR validation checklist — runs full test suite, verifies type hints, signal cleanup, no leaked TODOs, architecture compliance, and naming conventions. Also handles post-merge knowledge capture (lessons-learned, patterns) when developer says "capture lessons" or "post-merge cleanup". The full flow is in .ai-workflow/flows/pr-flow.md.
license: MIT
metadata:
  audience: developers
  workflow: task-development
  phase: pr
---

# pr-flow (OpenCode wrapper)

This skill runs the **PR validation** flow plus PR feedback handling and post-merge knowledge capture. Three modes: pre-pr, feedback, capture. Full flow at `.ai-workflow/flows/pr-flow.md`.

## When to invoke

- "Validate {task-name} for PR"
- "PR check for {task-name}"
- "Is {task-name} ready to merge?"
- "Process PR feedback for {task-name}"
- "Capture lessons learned for {task-name}"

## Pre-PR mode runs the full checklist

Pulls from `.ai-workflow/universal/rules.md`, the profile's `rules.md`, and `.ai-workflow/rules.md`. Verifies:

- All task flows for the task complete
- task-design.md signed off
- Tests pass (project's test command — never skip)
- No leaked `{slot}` placeholders (Rule 12)
- Naming conventions
- No architectural violations (project-specific rules)
- task-technical-design.md and task-edge-cases.md updated

## Hard rule

Never present a PR as "ready" with failing tests. The user's global rule "never skip any test" applies.

## Capture mode commits

When run in `capture` mode (post-merge knowledge capture), the final step is a CRITICAL-gate commit that stages `flow-storage/project/PATTERNS.md`, `flow-storage/project/DECISIONS.md`, and `flow-storage/tasks/{task-name}/lessons-learned.md`. Message: `docs(capture): {task-name} lessons learned and patterns`. Without this commit, lessons-learned sits in the working tree and gets lost on the next `git checkout` or merge. Opt-out via `--no-commit` to defer.

For full instructions read `.ai-workflow/flows/pr-flow.md`.
