---
name: pr-flow
description: Use when the developer says "validate X for PR", "PR check", "ready for PR", "pre-PR review", "open PR for X", or "is X ready to merge". Modal flow with three modes (pre-pr, feedback, capture) — each runs in 2 phases with 2 review gates [A]/[F]/[R] (one after the work is done, one before submit/commit). Pre-PR validation runs full test suite, verifies type hints, no leaked TODOs, architecture compliance, naming conventions. Also handles post-merge knowledge capture (lessons-learned, patterns) when developer says "capture lessons" or "post-merge cleanup". The full flow is in .ai-workflow/flows/pr-flow.md.
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

## What this flow does

**Modal: 3 modes, each with 2 phases and 2 gates.** Sub-tasks within each phase auto-proceed without intermediate gates. See `.ai-workflow/universal/workflow-structure.md` "PR Flow Modes".

### Mode: pre-pr (default)

- **Phase 1: VALIDATE** *(STANDARD)* — task-flow completeness, doc check, code quality, test execution, integration check. Pulls from universal/rules + profile/rules + project/rules.
- **Gate 1: Validation review** — developer sees PASS/FAIL across all checks.
- **Phase 2: REPORT** *(CRITICAL)* — generate PR description; OPEN PR (or print if `--dry-run`) AFTER gate is accepted.
- **Gate 2: PR submission review** — PR is publicly visible once accepted.

### Mode: feedback

- **Phase 1: PROCESS** *(STANDARD)* — per-comment loop: AI analyzes, creates feedback task flow, suggests fix, applies if approved.
- **Gate 1: Feedback review** — developer sees all processed comments + fixes applied/skipped/queued.
- **Phase 2: COMMIT** *(CRITICAL)* — stage feedback task flows + code fixes; commit AFTER gate is accepted.
- **Gate 2: Commit review** — opt-out: `--no-commit`.

### Mode: capture

- **Phase 1: ANALYZE** *(STANDARD)* — read all task flows; generate lessons-learned; propose KB updates (PATTERNS, DECISIONS, CONVENTIONS); update task catalog + living docs; capture metrics.
- **Gate 1: Capture review** — developer sees lessons + proposed KB diffs + metrics together.
- **Phase 2: COMMIT** *(CRITICAL)* — stage `flow-storage/project/PATTERNS.md`, `flow-storage/project/DECISIONS.md`, `flow-storage/tasks/{task-name}/docs/lessons-learned.md`; commit `docs(capture): {task-name} lessons learned and patterns` AFTER gate is accepted. Without this commit, lessons-learned sits in the working tree and gets lost on the next `git checkout`. Opt-out: `--no-commit`.
- **Gate 2: Commit review** — presented BEFORE the git commit executes.

## Hard rules

- Never present a PR as "ready" with failing tests. The user's global rule "never skip any test" applies.
- Sub-tasks within a phase auto-proceed; they are NOT user gates. The 2-gate-per-mode cadence is deliberate.

For full instructions read `.ai-workflow/flows/pr-flow.md`.
