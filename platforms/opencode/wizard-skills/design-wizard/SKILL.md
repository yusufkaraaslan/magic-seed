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

Six phases, each with an explicit gate type (LIGHT auto-proceed, STANDARD `[A]/[F]/[R]`, or CRITICAL `[A]/[F]/[R]` + checklist). See `.ai-workflow/universal/workflow-structure.md` "Phase Gate Protocol" for the full spec.

1. **UNDERSTAND** *(LIGHT — auto-proceed)* — Load project context. No gate; agent reports loaded files and proceeds.
2. **DESIGN** *(STANDARD)* — Produce diagrams (class, package, sequence at minimum) per `.ai-workflow/universal/diagram-standards.md`. **Render every diagram source to `.svg` (or `.png` fallback) and commit the image alongside the source.** A `.puml` without a matching image is not a deliverable. If no renderer is installed locally, stop the phase and instruct the developer to install one (commands in diagram-standards.md "Local Rendering"); do not present the gate with unrendered sources.
3. **SPECIFY** *(STANDARD)* — Write `docs/features/{name}/{DESIGN,TDD,EDGE-CASES}.md`.
4. **PLAN** *(CRITICAL — checklist gate)* — Decompose into implementation issues in `docs/features/{name}/issues/`. Before [A]ccept, verify: temporal contradictions in deps, undefined types, coverage gaps, parallel issues sharing files, oversized issues.
5. **FINALIZE** *(CRITICAL — immutability gate)* — Sign off DESIGN.md (Rule 9: immutable after sign-off). Last chance to change DESIGN.md prose; post sign-off, deviations go in TDD.md only.
6. **COMMIT** *(CRITICAL — commit gate)* — Stage `docs/features/{name}/` and run `git commit` with message `design({name}): sign off DESIGN.md and {N} issues`. Without this commit, the "Immutable: Yes" claim in DESIGN.md is convention-only — anyone can still edit the file. Locking it in git history is what makes the immutability load-bearing. Opt-out: developer can pass `--no-commit` to skip this phase when bundling with adjacent work.

## Hard rules

- Do not write any feature code from this wizard. Implementation is the implement-wizard's job.
- Do not auto-accept any phase gate (Rule 6). Always present `[A]/[F]/[R]` to the developer.
- Read `.ai-workflow/universal/rules.md` and `.ai-workflow/rules.md` before starting; they're non-negotiable.

For full instructions read `.ai-workflow/wizards/design-wizard.md`.
