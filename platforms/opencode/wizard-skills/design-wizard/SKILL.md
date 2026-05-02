---
name: design-wizard
description: MANDATORY before writing any feature code. Use when the developer says "design a feature", "design X", "let's design X", "plan feature X", or "I want to design Y". Produces DESIGN.md, TDD.md, EDGE-CASES.md, diagrams, and implementation issues for a feature, with phase review gates [A]/[F]/[R] at every step. Refuses to skip phases. The full flow is in .ai-workflow/wizards/design-wizard.md — load that file and execute it. Without a signed-off DESIGN.md from this wizard, no feature code may be written (project Rule 9).
license: MIT
metadata:
  audience: developers
  workflow: feature-development
  phase: design
---

# design-wizard (OpenCode wrapper)

This skill runs the **design phase** of magic-seed for a feature. The full multi-phase flow lives in `.ai-workflow/wizards/design-wizard.md` in the project. **Read that file and execute it phase by phase.**

## When to invoke

Whenever the developer expresses any of:

- "Design a feature called X"
- "Let's design X"
- "Plan the feature X"
- "I want to start working on X" (with no prior DESIGN.md)
- An implementation request for a feature that has no DESIGN.md yet — run design-wizard first, then implement-wizard

## What this wizard does

Five phases, each ending with `[A]ccept / [F]eedback / [R]eject`:

1. **UNDERSTAND** — Load project context (`.ai-workflow/universal/`, `.ai-workflow/profiles/{detected}/`, `docs/project/*`)
2. **DESIGN** — Produce diagrams (class, package, sequence at minimum) per `.ai-workflow/universal/diagram-standards.md`
3. **SPECIFY** — Write `docs/features/{name}/{DESIGN,TDD,EDGE-CASES}.md`
4. **PLAN** — Decompose into implementation issues in `docs/features/{name}/issues/`
5. **FINALIZE** — Sign off DESIGN.md (Rule 9: immutable after sign-off)

## Hard rules

- Do not write any feature code from this wizard. Implementation is the implement-wizard's job.
- Do not auto-accept any phase gate (Rule 6). Always present `[A]/[F]/[R]` to the developer.
- Read `.ai-workflow/universal/rules.md` and `.ai-workflow/rules.md` before starting; they're non-negotiable.

For full instructions read `.ai-workflow/wizards/design-wizard.md`.
