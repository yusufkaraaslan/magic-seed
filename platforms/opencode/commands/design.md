---
description: Design a feature — DESIGN.md, TDD.md, diagrams, and implementation issues with phase review gates
agent: build
---

Run the **design-wizard** for: $ARGUMENTS

Load the `design-wizard` skill (or read `.ai-workflow/wizards/design-wizard.md` directly if the skill is unavailable) and execute its six phases in order:

1. **UNDERSTAND** *(LIGHT — auto-proceed)* — Load project context from `.ai-workflow/universal/`, `.ai-workflow/profiles/{detected}/`, `docs/project/*`, and the project's design docs.
2. **DESIGN** *(STANDARD)* — Produce class, package, and sequence diagrams. Render every `.puml` to `.svg` (or `.png`); a source without an image is not a deliverable.
3. **SPECIFY** *(STANDARD)* — Write `docs/features/{feature}/{DESIGN,TDD,EDGE-CASES}.md`.
4. **PLAN** *(CRITICAL — checklist gate)* — Decompose into implementation issues. Verify: temporal contradictions in deps, undefined types, coverage gaps, parallel issues sharing files, oversized issues.
5. **FINALIZE** *(CRITICAL — immutability gate)* — Sign off DESIGN.md (Rule 9: immutable after sign-off).
6. **COMMIT** *(CRITICAL)* — Stage `docs/features/{feature}/` and run `git commit` with message `design({feature}): sign off DESIGN.md and {N} issues`. Skip this phase if `--no-commit` was passed.

Hard rules:
- Do not auto-accept any gate. Always present `[A]/[F]/[R]` to the developer.
- Do not write any feature code from this command — that's the implement-wizard's job.
- Universal rules in `.ai-workflow/universal/rules.md` and project rules in `.ai-workflow/rules.md` are non-negotiable.

If $ARGUMENTS is empty, ask the developer for the feature name (kebab-case) before proceeding.
