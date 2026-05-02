---
name: deploy-flow
description: Use when the developer says "deploy X", "build X", "ship X", "release X", "export X", "make a release build", or "build for production". Build verification, smoke tests, and export packaging for the project's target platform (Godot export presets, npm build, Python wheel, Docker image, etc.) in 2 phases with 2 CRITICAL review gates [A]/[F]/[R] (one before deploy executes, one before announcement goes out). Both gates are CRITICAL because deploy is hard-to-reverse and announcement is publicly irreversible. The full flow is in .ai-workflow/flows/deploy-flow.md.
license: MIT
metadata:
  audience: developers
  workflow: task-development
  phase: deploy
---

# deploy-flow (OpenCode wrapper)

This skill runs build verification and packaging for the project's target platform. Full flow at `.ai-workflow/flows/deploy-flow.md`.

## When to invoke

- "Deploy {task-name}"
- "Build a release"
- "Ship {task-name}"
- "Export the game"
- "Make a production build"

## What this flow does

**2 phases, 2 CRITICAL gates.** Each phase contains multiple sub-tasks that auto-proceed without intermediate gates. Both gates are CRITICAL because each represents a hard-to-reverse decision. See `.ai-workflow/universal/workflow-structure.md` "Phase Architecture".

### Phase 1: PREPARE *(GATE TYPE C — CRITICAL)*

Sub-tasks (auto-proceed):
- **1.1 CHECKLIST** — Verify task complete, docs in order, tests pass, no debug code, no secrets in artifacts.
- **1.2 VERSION** — Determine version bump; update version files + CHANGELOG.md; generate release notes.
- **1.3 BUILD** — Build with the project's tooling; verify artifact size + signatures; smoke test.

→ **Gate 1: Ready to deploy?** — last reversible point. Sub-task 2.1 (the actual deploy) executes only after this gate is accepted.

### Phase 2: SHIP *(GATE TYPE C — CRITICAL)*

Sub-tasks (auto-proceed):
- **2.1 DEPLOY** *(executes only after Gate 1 is accepted)* — Push artifact to target environment; monitor for errors.
- **2.2 VERIFY** — Health checks, performance vs baseline, hold for soak window if profile requires. If anything regresses → roll back; do not proceed to 2.3.
- **2.3 ANNOUNCE** *(executes only after Gate 2 is accepted)* — Send release notes to configured channels; update status page.

→ **Gate 2: All clear?** — announcement is **public**. Roll back instead of accept if metrics regressed.

## Hard rules

- **Tests must pass before build.** No "build despite failing tests" path.
- **Artifact verification is mandatory** — don't declare deploy success without a smoke test.

For full instructions read `.ai-workflow/flows/deploy-flow.md`.
