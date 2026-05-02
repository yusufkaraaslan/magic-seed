---
name: implement-flow
description: Use when the developer says "implement X", "implement task flow Y for X", "work on the next task flow", "continue X", "next task flow", or asks to write task code. Implements ONE task flow from a task that already has a signed-off task-design.md, in 2 phases with 2 review gates [A]/[F]/[R] (one after build+validate, one before commit). REFUSES to run if flow-storage/tasks/{task-name}/design/task-design.md does not exist or is not signed off — run design-flow first in that case. The full flow is in .ai-workflow/flows/implement-flow.md.
license: MIT
metadata:
  audience: developers
  workflow: task-development
  phase: implement
---

# implement-flow (OpenCode wrapper)

This skill runs the **implementation phase** for one task flow of a task. The full flow lives in `.ai-workflow/flows/implement-flow.md`. Read it and execute.

## When to invoke

- "Implement the next task flow for {task-name}"
- "Implement {task-name}"
- "Work on task flow {number} for {task-name}"
- "Continue implementing {task-name}"

## Hard preconditions

Before doing anything:

1. **task-design.md must exist** at `flow-storage/tasks/{task-name}/design/task-design.md`
2. **task-design.md must be signed off** (status `v1.0 — Signed Off`, immutable per Rule 9)
3. **An task flow file must exist** at `flow-storage/tasks/{task-name}/implement/flow-plan/{NN}-{name}.md` with `status: ready`

If any precondition fails, **STOP** and either:

- Tell the developer to run **design-flow** first (no task-design.md), OR
- Tell them to complete a [F]eedback iteration on the design (DESIGN not signed off), OR
- Ask which task flow to work on (no `status: ready` task flow)

## What this flow does

**2 phases, 2 gates.** Each phase contains multiple sub-tasks that auto-proceed without intermediate gates. See `.ai-workflow/universal/workflow-structure.md` "Phase Architecture".

### Phase 1: BUILD *(GATE TYPE B — STANDARD)*

Sub-tasks (auto-proceed):
- **1.1 READ** — Load context, task flow file, design docs, prior completed task flows, reference implementations.
- **1.2 PLAN** — Analyze acceptance criteria, map to existing patterns, structure the implementation.
- **1.3 IMPLEMENT** — Write code incrementally per profile conventions, applying universal + project rules.
- **1.4 AUTO-VALIDATE** — Run profile checks + project test suite (NEVER skip tests). If failures, surface at the gate; do not auto-proceed.
- **1.5 DOC-SYNC** — Update `task-technical-design.md` with deviations (task-design.md itself stays immutable per Rule 9), update `task-edge-cases.md`, update `flow-storage/project/PATTERNS.md` if a new pattern emerged.

→ **Gate 1: Implementation review** — developer sees files changed, validation results, doc updates together.

### Phase 2: COMMIT *(GATE TYPE C — CRITICAL)*

Sub-tasks (auto-proceed):
- **2.1 UPDATE TASK FLOW** — Transform task flow file into knowledge record (status: complete, accepted-date, implementation notes, code refs).
- **2.2 COMMIT** *(executes only after the gate is accepted)* — Stage only the files this task flow produced; conventional commit message; `git commit`.

→ **Gate 2: Commit review** — presented BEFORE 2.2 executes git commit. Opt-out: `--no-commit` skips sub-task 2.2 only.

Code goes in the project tree per the profile's conventions (e.g. `systems/`, `src/`, etc.). Tests in the project's test directory.

For full instructions read `.ai-workflow/flows/implement-flow.md`.
