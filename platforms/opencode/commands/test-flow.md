---
description: Run or add tests for a task using the project's test framework
agent: build
---

Run the **test-flow** for: $ARGUMENTS

The argument is one of:
- `<task>` — run/add tests for that task
- `<file-or-system>` — target a specific module
- (empty) — run the entire test suite for the project

Load the `test-flow` skill (or read `.ai-workflow/flows/test-flow.md` directly) and execute its **2 phases**. Each phase contains sub-tasks that auto-proceed without intermediate gates. test-flow does NOT git commit — tests are committed via implement-flow or pr-flow alongside the code they cover:

### Phase 1: WRITE *(STANDARD gate)*
- **1.1 READ** — Find the test framework (GUT for Godot, pytest for Python, jest/vitest for JS, etc.); load task docs + implementation; identify testable units; check existing tests.
- **1.2 PLAN** — Map each acceptance criterion + edge case to test case(s); identify mocks/fixtures.
- **1.3 GENERATE** — Create test files following project conventions; write stubs.
- **1.4 IMPLEMENT** — Fill in test bodies (unit + integration + edge case).
- → **Gate 1: Test code review** — outline + stubs + filled tests together. No tests run yet.

### Phase 2: VERIFY *(STANDARD gate)*
- **2.1 VALIDATE** — Run the project's test suite (e.g. `godot --path . -s addons/gut/gut_cmdln.gd ...`). **Never skip, never partial-run.** Capture pass/fail + coverage delta. If failures, surface at the gate; do not paper over.
- **2.2 DOCUMENT** — Append summary to `flow-storage/tasks/{task-name}/test/`; update TDD + task flow file + PATTERNS.md if new patterns emerged.
- → **Gate 2: Results review** — test results, coverage delta, new edge cases.

Hard rules — apply throughout:
- **Never skip a test.** User global rule. If a test is broken, fix it; if it's wrong, run feedback with the developer; never delete or skip silently.
- **Pure systems must be tested.** Profile/project may declare which systems are pure; those need unit tests (project rule, e.g. RK7 for Reversi King).
- **Don't mock what you can integration-test.** Mocks should be a last resort.

If the test runner isn't installed/available in the current shell, STOP and tell the developer the install command for their OS.
