---
name: implement-wizard
description: Use when the developer says "implement X", "implement issue Y for X", "work on the next issue", "continue X", "next issue", or asks to write feature code. Implements ONE issue from a feature that already has a signed-off DESIGN.md. REFUSES to run if docs/features/{feature}/DESIGN.md does not exist or is not signed off — run design-wizard first in that case. The full flow is in .ai-workflow/wizards/implement-wizard.md.
license: MIT
metadata:
  audience: developers
  workflow: feature-development
  phase: implement
---

# implement-wizard (OpenCode wrapper)

This skill runs the **implementation phase** for one issue of a feature. The full flow lives in `.ai-workflow/wizards/implement-wizard.md`. Read it and execute.

## When to invoke

- "Implement the next issue for {feature}"
- "Implement {feature}"
- "Work on issue {number} for {feature}"
- "Continue implementing {feature}"

## Hard preconditions

Before doing anything:

1. **DESIGN.md must exist** at `docs/features/{feature}/DESIGN.md`
2. **DESIGN.md must be signed off** (status `v1.0 — Signed Off`, immutable per Rule 9)
3. **An issue file must exist** at `docs/features/{feature}/issues/{NN}-{name}.md` with `status: ready`

If any precondition fails, **STOP** and either:

- Tell the developer to run **design-wizard** first (no DESIGN.md), OR
- Tell them to complete a [F]eedback iteration on the design (DESIGN not signed off), OR
- Ask which issue to work on (no `status: ready` issue)

## What this wizard does

Phase 1 LOAD → Phase 2 IMPLEMENT → Phase 3 TEST → Phase 4 INTEGRATE → Phase 5 REVIEW. Each phase ends with `[A]ccept / [F]eedback / [R]eject`. Code goes in the project tree per the profile's conventions (e.g. `systems/`, `src/`, etc.). Tests in the project's test directory. Updates `TDD.md` with deviations from DESIGN.md (DESIGN.md itself stays immutable).

For full instructions read `.ai-workflow/wizards/implement-wizard.md`.
