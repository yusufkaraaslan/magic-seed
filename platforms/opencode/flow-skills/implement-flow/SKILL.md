---
name: implement-flow
description: Use when the developer says "implement X", "implement issue Y for X", "work on the next issue", "continue X", "next issue", or asks to write task code. Implements ONE issue from a task that already has a signed-off task-design.md. REFUSES to run if flow-storage/tasks/{task-name}/design/task-design.md does not exist or is not signed off — run design-flow first in that case. The full flow is in .ai-workflow/flows/implement-flow.md.
license: MIT
metadata:
  audience: developers
  workflow: task-development
  phase: implement
---

# implement-flow (OpenCode wrapper)

This skill runs the **implementation phase** for one issue of a task. The full flow lives in `.ai-workflow/flows/implement-flow.md`. Read it and execute.

## When to invoke

- "Implement the next task flow for {task-name}"
- "Implement {task-name}"
- "Work on issue {number} for {task-name}"
- "Continue implementing {task-name}"

## Hard preconditions

Before doing anything:

1. **task-design.md must exist** at `flow-storage/tasks/{task-name}/design/task-design.md`
2. **task-design.md must be signed off** (status `v1.0 — Signed Off`, immutable per Rule 9)
3. **An issue file must exist** at `flow-storage/tasks/{task-name}/implement/flow-plan/{NN}-{name}.md` with `status: ready`

If any precondition fails, **STOP** and either:

- Tell the developer to run **design-flow** first (no task-design.md), OR
- Tell them to complete a [F]eedback iteration on the design (DESIGN not signed off), OR
- Ask which issue to work on (no `status: ready` issue)

## What this flow does

Phase 1 LOAD → Phase 2 IMPLEMENT → Phase 3 TEST → Phase 4 INTEGRATE → Phase 5 REVIEW. Each phase ends with `[A]ccept / [F]eedback / [R]eject`. Code goes in the project tree per the profile's conventions (e.g. `systems/`, `src/`, etc.). Tests in the project's test directory. Updates `task-technical-design.md` with deviations from task-design.md (task-design.md itself stays immutable).

For full instructions read `.ai-workflow/flows/implement-flow.md`.
