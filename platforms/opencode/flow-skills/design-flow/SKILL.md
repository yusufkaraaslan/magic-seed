---
name: design-flow
description: MANDATORY before writing any task code. Use when the developer says "design a task", "design X", "let's design X", "plan task X", or "I want to design Y". Produces task-design.md, task-technical-design.md, task-edge-cases.md, diagrams, and implementation task flows for a task, with phase review gates [A]/[F]/[R] at every step. Refuses to skip phases. The full flow is in .ai-workflow/flows/design-flow.md — load that file and execute it. Without a signed-off task-design.md from this flow, no task code may be written (project Rule 9).
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

Six phases, each with an explicit gate type (LIGHT auto-proceed, STANDARD `[A]/[F]/[R]`, or CRITICAL `[A]/[F]/[R]` + checklist). See `.ai-workflow/universal/workflow-structure.md` "Phase Gate Protocol" for the full spec.

1. **UNDERSTAND** *(LIGHT — auto-proceed)* — Load project context. No gate; agent reports loaded files and proceeds.
2. **DESIGN** *(STANDARD)* — Produce diagrams (class, package, sequence at minimum) per `.ai-workflow/universal/diagram-standards.md`. **Render every diagram source to `.svg` (or `.png` fallback) and commit the image alongside the source.** A `.puml` without a matching image is not a deliverable. If no renderer is installed locally, stop the phase and instruct the developer to install one (commands in diagram-standards.md "Local Rendering"); do not present the gate with unrendered sources.
3. **SPECIFY** *(STANDARD)* — Write `flow-storage/tasks/{task-name}/{task-design,task-technical-design,task-edge-cases}.md`.
4. **PLAN** *(CRITICAL — checklist gate)* — Decompose into implementation task flows in `flow-storage/tasks/{task-name}/implement/flow-plan/`. Before [A]ccept, verify: temporal contradictions in deps, undefined types, coverage gaps, parallel task flows sharing files, oversized task flows.
5. **FINALIZE** *(CRITICAL — immutability gate)* — Sign off task-design.md (Rule 9: immutable after sign-off). Last chance to change task-design.md prose; post sign-off, deviations go in task-technical-design.md only.
6. **COMMIT** *(CRITICAL — commit gate)* — Stage `flow-storage/tasks/{task-name}/` and run `git commit` with message `design({name}): sign off task-design.md and {N} task flows`. Without this commit, the "Immutable: Yes" claim in task-design.md is convention-only — anyone can still edit the file. Locking it in git history is what makes the immutability load-bearing. Opt-out: developer can pass `--no-commit` to skip this phase when bundling with adjacent work.

## Hard rules

- Do not write any task code from this flow. Implementation is the implement-flow's job.
- Do not auto-accept any phase gate (Rule 6). Always present `[A]/[F]/[R]` to the developer.
- Read `.ai-workflow/universal/rules.md` and `.ai-workflow/rules.md` before starting; they're non-negotiable.

For full instructions read `.ai-workflow/flows/design-flow.md`.
