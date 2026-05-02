---
description: Design a task — task-design.md, task-technical-design.md, diagrams, and implementation task flows in 2 phases with 2 review gates
agent: build
---

Run the **design-flow** for: $ARGUMENTS

Load the `design-flow` skill (or read `.ai-workflow/flows/design-flow.md` directly if the skill is unavailable) and execute its **2 phases** in order. Each phase contains sub-tasks that auto-proceed without intermediate gates:

### Phase 1: SPECIFY *(STANDARD gate)*
- **1.1 UNDERSTAND** — Load project context from `.ai-workflow/universal/`, `.ai-workflow/profiles/{detected}/`, `flow-storage/project/*`, and the project's design docs.
- **1.2 DESIGN** — Produce class, package, and sequence diagrams. Render every `.puml` to `.svg` (or `.png`); a source without an image is not a deliverable.
- **1.3 SPECIFY** — Write `flow-storage/tasks/{task-name}/design/{task-design,task-technical-design,task-edge-cases}.md`.
- → **Gate 1: Design package review** — diagrams + spec docs together.

### Phase 2: COMMIT *(CRITICAL gate)*
- **2.1 PLAN** — Decompose into implementation task flows in `implement/flow-plan/`. Run internal validation (temporal contradictions in deps, undefined types, coverage gaps, parallel task flows sharing files, oversized task flows). Surface failures at the gate.
- **2.2 FINALIZE** — Sign off task-design.md (Rule 9: immutable after sign-off).
- **2.3 COMMIT** *(executes only after the gate is accepted)* — Stage `flow-storage/tasks/{task-name}/` and run `git commit` with message `design({task-name}): sign off task-design.md and {N} task flows`. Skip 2.3 only if `--no-commit` was passed.
- → **Gate 2: Plan + final approval** — presented BEFORE 2.3 executes git commit.

Hard rules:
- Do not auto-accept any of the 2 gates (Rule 6). Always present `[A]/[F]/[R]` to the developer.
- Sub-tasks within a phase auto-proceed; they are NOT user gates.
- Do not write any task code from this command — that's the implement-flow's job.
- Universal rules in `.ai-workflow/universal/rules.md` and project rules in `.ai-workflow/rules.md` are non-negotiable.

If $ARGUMENTS is empty, ask the developer for the task name (kebab-case) before proceeding.
