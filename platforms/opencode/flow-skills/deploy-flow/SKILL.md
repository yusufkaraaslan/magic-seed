---
name: deploy-flow
description: Use when the developer says "deploy X", "build X", "ship X", "release X", "export X", "make a release build", or "build for production". Build verification, smoke tests, and export packaging for the project's target platform (Godot export presets, npm build, Python wheel, Docker image, etc.). The full flow is in .ai-workflow/flows/deploy-flow.md.
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

1. Runs the full test suite first (pre-deploy gate)
2. Builds with the project's build/export tooling
3. Smoke tests the build artifact (launches it, verifies basic startup)
4. Reports success/failure with the artifact path

## Hard rules

- **Tests must pass before build.** No "build despite failing tests" path.
- **Artifact verification is mandatory** — don't declare deploy success without a smoke test.

For full instructions read `.ai-workflow/flows/deploy-flow.md`.
