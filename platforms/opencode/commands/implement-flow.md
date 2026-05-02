---
description: Implement one task flow from a designed task — code, tests, doc-sync, commit
agent: build
---

Run the **implement-flow** for: $ARGUMENTS

The argument is one of:
- `<task>` — implement the next pending task flow for that task
- `<task> <task-flow-number-or-name>` — implement that specific task flow
- (empty) — ask the developer which task/task-flow

Load the `implement-flow` skill (or read `.ai-workflow/flows/implement-flow.md` directly) and execute its eight phases:

1. **READ** *(LIGHT)* — Load issue file, task-design.md, task-technical-design.md sections, reference implementations.
2. **PLAN** *(STANDARD)* — Map acceptance criteria to existing patterns; surface ambiguities.
3. **IMPLEMENT** *(STANDARD)* — Write code per profile + project conventions.
4. **AUTO-VALIDATE** *(STANDARD)* — Run the project test suite (NEVER skip tests). If the test runner isn't available locally, STOP the phase and tell the developer — do not skip ahead to review.
5. **DOC-SYNC** *(STANDARD)* — Update task-technical-design.md with deviations from task-design.md (task-design.md itself stays immutable per Rule 9).
6. **DEVELOPER REVIEW** *(STANDARD)* — Present files changed, tests run, coverage.
7. **UPDATE ISSUE** *(LIGHT)* — Mark issue complete with `accepted-date`.
8. **COMMIT** *(CRITICAL)* — Stage only this issue's files; commit per conventional commits with issue reference. Skip if `--no-commit`.

Hard preconditions — **STOP** and refuse to proceed if any fail:
- `flow-storage/tasks/{task-name}/design/task-design.md` exists and is signed off (status: v1.0 — Signed Off, Immutable: Yes). If not, tell the developer to run `/design {task-name}` first.
- The target issue file exists with `status: pending` or `status: in_progress`.
- The user's global rule "never skip any test" applies to Phase 4 — failing tests block the flow, do not silently bypass.

If preconditions fail, do not improvise task code. Stop and ask the developer for direction.
