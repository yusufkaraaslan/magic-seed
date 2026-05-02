---
description: Build verification, smoke tests, and packaging for the project's target platform
agent: build
---

Run the **deploy-flow** for: $ARGUMENTS

The argument is the task or release scope (or empty for "current main").

Load the `deploy-flow` skill (or read `.ai-workflow/flows/deploy-flow.md` directly) and execute its **2 phases**. Each phase contains sub-tasks that auto-proceed without intermediate gates. Both gates are CRITICAL because deploy is hard-to-reverse and announcement is publicly irreversible:

### Phase 1: PREPARE *(CRITICAL gate)*
- **1.1 CHECKLIST** — Verify task complete, docs in order, **full test suite passes** (NEVER skip — user global rule applies), no debug code, no secrets in artifacts.
- **1.2 VERSION** — Determine version bump; update version files + CHANGELOG.md; generate release notes.
- **1.3 BUILD** — Build with the project's tooling (Godot export presets, npm build, Python wheel, Docker image, etc.); verify artifact size + signatures; smoke test the build artifact (launch it, verify basic startup).
- → **Gate 1: Ready to deploy?** — last reversible point. Sub-task 2.1 (the actual deploy) executes only after this gate is accepted.

### Phase 2: SHIP *(CRITICAL gate)*
- **2.1 DEPLOY** *(executes only after Gate 1 is accepted)* — Push artifact to target environment; monitor for errors.
- **2.2 VERIFY** — Health checks, performance vs baseline, hold for soak window if profile requires. If anything regresses → roll back; do not proceed to 2.3.
- **2.3 ANNOUNCE** *(executes only after Gate 2 is accepted)* — Send release notes to configured channels.
- → **Gate 2: All clear?** — announcement is **public**. Roll back instead of accept if metrics regressed.

Hard rules:
- **Tests must pass before build.** Pre-deploy gate is non-negotiable.
- **Smoke test is mandatory.** A build that hasn't been launched isn't verified.
- **No silent fallback to a prior good build.** If this build fails, surface the failure; do not deploy a stale artifact.

If the build/export tooling isn't available locally, STOP and tell the developer the install command.
