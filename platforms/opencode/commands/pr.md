---
description: Pre-PR validation, PR feedback handling, or post-merge knowledge capture
agent: build
---

Run the **pr-wizard** for: $ARGUMENTS

The argument controls mode:
- `<feature>` (default) — pre-PR validation: tests pass, type hints, signal cleanup, no leaked TODOs, architecture compliance, naming conventions
- `<feature> feedback` — PR feedback mode: developer pastes review comments, wizard categorizes and proposes fixes
- `<feature> capture` — post-merge knowledge capture: lessons-learned, PATTERNS.md, DECISIONS.md updates, then commit

Load the `pr-wizard` skill (or read `.ai-workflow/wizards/pr-wizard.md` directly).

**pre-pr mode** (default):
1. Issue completeness — every issue for the feature has `status: complete` and a sensible `accepted-date`
2. DESIGN.md signed off
3. Tests pass — full suite, never partial. If the test runner isn't available, STOP and tell the developer.
4. No leaked `{slot}` placeholders (Rule 12)
5. Naming conventions and architecture compliance per profile + project rules
6. TDD.md and EDGE-CASES.md updated with implementation discoveries
7. Report PASS / FAIL with details

**capture mode** (post-merge): runs Steps 1–7 above followed by:
8. **COMMIT** *(CRITICAL)* — Stage `docs/project/PATTERNS.md`, `docs/project/DECISIONS.md`, `docs/features/{feature}/lessons-learned.md`. Commit message: `docs(capture): {feature} lessons learned and patterns`. Skip if `--no-commit`.

Hard rule: **never present a PR as ready with failing tests.** User global rule "never skip any test" applies.

If $ARGUMENTS is empty, ask which feature and which mode.
