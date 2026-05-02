---
description: Design a task — task-design.md, task-technical-design.md, diagrams, and implementation task flows with phase review gates
agent: build
---

Run the **design-flow** for: $ARGUMENTS

Load the `design-flow` skill (or read `.ai-workflow/flows/design-flow.md` directly if the skill is unavailable) and execute its six phases in order:

1. **UNDERSTAND** *(LIGHT — auto-proceed)* — Load project context from `.ai-workflow/universal/`, `.ai-workflow/profiles/{detected}/`, `flow-storage/project/*`, and the project's design docs.
2. **DESIGN** *(STANDARD)* — Produce class, package, and sequence diagrams. Render every `.puml` to `.svg` (or `.png`); a source without an image is not a deliverable.
3. **SPECIFY** *(STANDARD)* — Write `flow-storage/tasks/{task-name}/{task-design,task-technical-design,task-edge-cases}.md`.
4. **PLAN** *(CRITICAL — checklist gate)* — Decompose into implementation task flows. Verify: temporal contradictions in deps, undefined types, coverage gaps, parallel task flows sharing files, oversized task flows.
5. **FINALIZE** *(CRITICAL — immutability gate)* — Sign off task-design.md (Rule 9: immutable after sign-off).
6. **COMMIT** *(CRITICAL)* — Stage `flow-storage/tasks/{task-name}/` and run `git commit` with message `design({task-name}): sign off task-design.md and {N} task flows`. Skip this phase if `--no-commit` was passed.

Hard rules:
- Do not auto-accept any gate. Always present `[A]/[F]/[R]` to the developer.
- Do not write any task code from this command — that's the implement-flow's job.
- Universal rules in `.ai-workflow/universal/rules.md` and project rules in `.ai-workflow/rules.md` are non-negotiable.

If $ARGUMENTS is empty, ask the developer for the task name (kebab-case) before proceeding.
