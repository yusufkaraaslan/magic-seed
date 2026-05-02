---
description: Implement one issue from a designed feature — code, tests, doc-sync, commit
agent: build
---

Run the **implement-wizard** for: $ARGUMENTS

The argument is one of:
- `<feature>` — implement the next pending issue for that feature
- `<feature> <issue-number-or-name>` — implement that specific issue
- (empty) — ask the developer which feature/issue

Load the `implement-wizard` skill (or read `.ai-workflow/wizards/implement-wizard.md` directly) and execute its eight phases:

1. **READ** *(LIGHT)* — Load issue file, DESIGN.md, TDD.md sections, reference implementations.
2. **PLAN** *(STANDARD)* — Map acceptance criteria to existing patterns; surface ambiguities.
3. **IMPLEMENT** *(STANDARD)* — Write code per profile + project conventions.
4. **AUTO-VALIDATE** *(STANDARD)* — Run the project test suite (NEVER skip tests). If the test runner isn't available locally, STOP the phase and tell the developer — do not skip ahead to review.
5. **DOC-SYNC** *(STANDARD)* — Update TDD.md with deviations from DESIGN.md (DESIGN.md itself stays immutable per Rule 9).
6. **DEVELOPER REVIEW** *(STANDARD)* — Present files changed, tests run, coverage.
7. **UPDATE ISSUE** *(LIGHT)* — Mark issue complete with `accepted-date`.
8. **COMMIT** *(CRITICAL)* — Stage only this issue's files; commit per conventional commits with issue reference. Skip if `--no-commit`.

Hard preconditions — **STOP** and refuse to proceed if any fail:
- `docs/features/{feature}/DESIGN.md` exists and is signed off (status: v1.0 — Signed Off, Immutable: Yes). If not, tell the developer to run `/design {feature}` first.
- The target issue file exists with `status: pending` or `status: in_progress`.
- The user's global rule "never skip any test" applies to Phase 4 — failing tests block the wizard, do not silently bypass.

If preconditions fail, do not improvise feature code. Stop and ask the developer for direction.
