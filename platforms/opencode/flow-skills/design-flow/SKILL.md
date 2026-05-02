---
name: design-flow
description: MANDATORY before writing any task code. Use when the developer says "design a task", "design X", "let's design X", "plan task X", or "I want to design Y". Produces task-design.md, task-technical-design.md, task-edge-cases.md, diagrams, and implementation task flows for a task in 2 phases with 2 review gates [A]/[F]/[R] (one after the design package is written, one before the final commit). Refuses to skip gates. The full flow is in .ai-workflow/flows/design-flow.md — load that file and execute it. Without a signed-off task-design.md from this flow, no task code may be written (project Rule 9).
license: MIT
metadata:
  audience: developers
  workflow: task-development
  phase: design
---

# design-flow (OpenCode wrapper)

This skill runs the **design phase** of ai-flow-anything for a task. The full multi-phase flow lives in `.ai-workflow/flows/design-flow.md` in the project. **Read that file and execute it phase by phase.**

## When to invoke

Whenever the developer expresses any of:

- "Design a task called X"
- "Let's design X"
- "Plan the task X"
- "I want to start working on X" (with no prior task-design.md)
- An implementation request for a task that has no task-design.md yet — run design-flow first, then implement-flow

## What this flow does

**2 phases, 2 gates.** Each phase contains multiple sub-tasks that auto-proceed without intermediate gates. See `.ai-workflow/universal/workflow-structure.md` "Phase Architecture" and "Phase Gate Protocol" for the full spec.

### Phase 1: SPECIFY *(GATE TYPE B — STANDARD)*

Sub-tasks (auto-proceed):
- **1.1 UNDERSTAND** — Load project context (universal/, profile/, flow-storage/project/).
- **1.2 DESIGN** — Produce diagrams (class, package, sequence at minimum) per `.ai-workflow/universal/diagram-standards.md`. **Render every diagram source to `.svg` (or `.png` fallback) and commit the image alongside the source.** A `.puml` without a matching image is not a deliverable. If no renderer is installed locally, stop and instruct the developer to install one before continuing.
- **1.3 SPECIFY** — Write `flow-storage/tasks/{task-name}/design/{task-design,task-technical-design,task-edge-cases}.md`.

→ **Gate 1: Design package review** — developer sees diagrams + the three spec docs together and forms one opinion.

### Phase 2: COMMIT *(GATE TYPE C — CRITICAL)*

Sub-tasks (auto-proceed):
- **2.1 PLAN** — Decompose into implementation task flows in `flow-storage/tasks/{task-name}/implement/flow-plan/`. Run internal validation (temporal contradictions in deps, undefined types, coverage gaps, parallel task flows sharing files, oversized task flows). Surface failures at the gate.
- **2.2 FINALIZE** — Sign off task-design.md (Rule 9: immutable after sign-off). Last chance to change task-design.md prose; post sign-off, deviations go in task-technical-design.md only.
- **2.3 COMMIT** *(executes only after the gate is accepted)* — Stage `flow-storage/tasks/{task-name}/` and run `git commit` with message `design({task-name}): sign off task-design.md and {N} task flows`. Locking it in git history is what makes the immutability load-bearing.

→ **Gate 2: Plan + final approval** — presented BEFORE 2.3 executes git commit. Accepting locks task-design.md immutable AND lands the commit. Opt-out: `--no-commit` skips sub-task 2.3 only (gate still presents).

## Hard rules

- Do not write any task code from this flow. Implementation is the implement-flow's job.
- Do not auto-accept any phase gate (Rule 6). Always present `[A]/[F]/[R]` to the developer at each of the 2 gates.
- Sub-tasks inside a phase auto-proceed; they are NOT user gates. The 2-gate cadence is deliberate.
- Read `.ai-workflow/universal/rules.md` and `.ai-workflow/rules.md` before starting; they're non-negotiable.

For full instructions read `.ai-workflow/flows/design-flow.md`.
