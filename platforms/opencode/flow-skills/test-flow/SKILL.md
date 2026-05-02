---
name: test-flow
description: Use when the developer says "test X", "run tests", "add tests for X", "test the task", "write tests", or "fix failing tests". Adds and runs tests using the project's test framework (GUT for Godot, pytest for Python, jest/vitest for JS, etc.) in 2 phases with 2 review gates [A]/[F]/[R] (one after tests are written, one after results are in). Enforces the user's global rule "never skip any test". The full flow is in .ai-workflow/flows/test-flow.md.
license: MIT
metadata:
  audience: developers
  workflow: task-development
  phase: test
---

# test-flow (OpenCode wrapper)

This skill adds and runs tests for a task. Full flow at `.ai-workflow/flows/test-flow.md`.

## When to invoke

- "Test {task-name}"
- "Add tests for {task-name}"
- "Run the tests"
- "Write tests for {file}"
- "Fix failing tests"

## What this flow does

**2 phases, 2 gates.** Each phase contains multiple sub-tasks that auto-proceed without intermediate gates. test-flow does NOT git commit — tests are committed via implement-flow or pr-flow alongside the code they cover.

### Phase 1: WRITE *(GATE TYPE B — STANDARD)*

Sub-tasks (auto-proceed):
- **1.1 READ** — Load task docs + implementation + identify testable units.
- **1.2 PLAN** — Map acceptance criteria + edge cases to test cases; identify mocks/fixtures.
- **1.3 GENERATE** — Create test files following project conventions; write stubs.
- **1.4 IMPLEMENT** — Fill in test bodies (unit + integration + edge case).

→ **Gate 1: Test code review** — developer sees outline + stubs + filled tests together. No tests run yet.

### Phase 2: VERIFY *(GATE TYPE B — STANDARD)*

Sub-tasks (auto-proceed):
- **2.1 VALIDATE** — Run the project's test framework (NEVER skip tests). Capture pass/fail + coverage delta. If failures, surface at the gate; do not paper over.
- **2.2 DOCUMENT** — Append summary to `flow-storage/tasks/{task-name}/test/`; update TDD + task flow file + PATTERNS.md if new patterns emerged.

→ **Gate 2: Results review** — developer sees test results, coverage delta, and any new edge cases.

## Hard rules

- **Never skip a test.** User global rule. If a test is broken, fix it; if it's wrong, run feedback with the developer; never delete or skip silently.
- **Pure systems must be tested.** Profile/project may declare which systems are pure; those need unit tests.
- **Don't mock what you can integration-test.** Mocks should be a last resort.

For full instructions read `.ai-workflow/flows/test-flow.md`.
