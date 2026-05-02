---
description: Run or add tests for a feature using the project's test framework
agent: build
---

Run the **test-wizard** for: $ARGUMENTS

The argument is one of:
- `<feature>` — run/add tests for that feature
- `<file-or-system>` — target a specific module
- (empty) — run the entire test suite for the project

Load the `test-wizard` skill (or read `.ai-workflow/wizards/test-wizard.md` directly) and execute its phases:

1. **IDENTIFY** — Find the test framework from the profile and project config (GUT for Godot, pytest for Python, jest/vitest for JS, etc.). Find existing tests for the target.
2. **GAP ANALYSIS** — Per project Rule 13 (test coverage), identify missing coverage for new functionality.
3. **WRITE TESTS** — If gaps exist, propose new test cases. Present to developer for review.
4. **RUN** — Execute the project's test suite using the documented command (e.g. `godot --path . -s addons/gut/gut_cmdln.gd ...`). **Never skip, never partial-run.**
5. **REPORT** — Present pass/fail counts. If failures, present them to the developer; do not paper over.

Hard rules — apply throughout:
- **Never skip a test.** User global rule. If a test is broken, fix it; if it's wrong, run feedback with the developer; never delete or skip silently.
- **Pure systems must be tested.** Profile/project may declare which systems are pure; those need unit tests (project rule, e.g. RK7 for Reversi King).
- **Don't mock what you can integration-test.** Mocks should be a last resort.

If the test runner isn't installed/available in the current shell, STOP and tell the developer the install command for their OS.
