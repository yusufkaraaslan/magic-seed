---
description: Pre-PR validation, PR feedback handling, or post-merge knowledge capture
agent: build
---

Run the **pr-flow** for: $ARGUMENTS

The argument controls mode:
- `<task>` (default) — pre-PR validation: tests pass, type hints, signal cleanup, no leaked TODOs, architecture compliance, naming conventions
- `<task> feedback` — PR feedback mode: developer pastes review comments, flow categorizes and proposes fixes
- `<task> capture` — post-merge knowledge capture: lessons-learned, PATTERNS.md, DECISIONS.md updates, then commit

Load the `pr-flow` skill (or read `.ai-workflow/flows/pr-flow.md` directly). Each mode runs in **2 phases with 2 gates**. Sub-tasks within each phase auto-proceed without intermediate gates.

### Mode: pre-pr (default)
- **Phase 1: VALIDATE** *(STANDARD)*
  - 1.1 Task flow completeness — every task flow has `status: complete` and `accepted-date`
  - 1.2 Documentation check — task-design.md signed off, no leaked `{slot}` placeholders (Rule 12)
  - 1.3 Code quality — naming conventions, architecture compliance per profile + project rules
  - 1.4 Test execution — full suite, never partial. If test runner isn't available, STOP and tell the developer.
  - 1.5 Integration check — task-technical-design.md and task-edge-cases.md updated with implementation discoveries
  - → **Gate 1: Validation review** — PASS/FAIL across all checks.
- **Phase 2: REPORT** *(CRITICAL)*
  - 2.1 Generate PR description from task flows + commits
  - 2.2 OPEN PR *(executes only after gate is accepted)* — or print description if `--dry-run`
  - → **Gate 2: PR submission review** — PR is publicly visible once accepted.

### Mode: feedback
- **Phase 1: PROCESS** *(STANDARD)* — per-comment loop: AI categorizes, creates feedback task flow, suggests fix, applies if approved.
  - → **Gate 1: Feedback review** — all processed comments + applied fixes.
- **Phase 2: COMMIT** *(CRITICAL)* — stage feedback task flows + code fixes; commit AFTER gate. Opt-out: `--no-commit`.
  - → **Gate 2: Commit review**.

### Mode: capture (post-merge)
- **Phase 1: ANALYZE** *(STANDARD)* — read all task flows; generate lessons-learned; propose KB updates (PATTERNS, DECISIONS, CONVENTIONS); update task catalog + living docs; capture metrics.
  - → **Gate 1: Capture review** — lessons + KB diffs + metrics together.
- **Phase 2: COMMIT** *(CRITICAL)* — stage `flow-storage/project/PATTERNS.md`, `flow-storage/project/DECISIONS.md`, `flow-storage/tasks/{task-name}/docs/lessons-learned.md`; commit `docs(capture): {task-name} lessons learned and patterns` AFTER gate. Opt-out: `--no-commit`.
  - → **Gate 2: Commit review**.

Hard rule: **never present a PR as ready with failing tests.** User global rule "never skip any test" applies.

If $ARGUMENTS is empty, ask which task and which mode.
