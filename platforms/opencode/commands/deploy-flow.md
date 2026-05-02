---
description: Build verification, smoke tests, and packaging for the project's target platform
agent: build
---

Run the **deploy-flow** for: $ARGUMENTS

The argument is the task or release scope (or empty for "current main").

Load the `deploy-flow` skill (or read `.ai-workflow/flows/deploy-flow.md` directly) and execute its phases:

1. **PRE-DEPLOY GATE** — Run the full test suite. If any failures, STOP. **No "build despite failing tests" path** — user global rule "never skip any test" applies.
2. **BUILD** — Use the project's build/export tooling (Godot export presets, npm build, Python wheel, Docker image, etc. — discover from profile + project config).
3. **SMOKE TEST** — Launch the build artifact, verify basic startup. Don't declare deploy success without this.
4. **REPORT** — Output the artifact path, version, smoke-test result.

Hard rules:
- **Tests must pass before build.** Pre-deploy gate is non-negotiable.
- **Smoke test is mandatory.** A build that hasn't been launched isn't verified.
- **No silent fallback to a prior good build.** If this build fails, surface the failure; do not deploy a stale artifact.

If the build/export tooling isn't available locally, STOP and tell the developer the install command.
