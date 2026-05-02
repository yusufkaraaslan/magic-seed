---
name: test-flow
description: Use when the developer says "test X", "run tests", "add tests for X", "test the task", "write tests", or "fix failing tests". Adds and runs tests using the project's test framework (GUT for Godot, pytest for Python, jest/vitest for JS, etc.). Enforces the user's global rule "never skip any test". The full flow is in .ai-workflow/flows/test-flow.md.
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

1. Identifies the project's test framework (from profile and project config)
2. Locates the existing tests for the task/files in question
3. Adds missing test coverage per project Rule 13 (test coverage for new tasks)
4. Runs the full test suite — **never skips, never partial-runs**
5. If tests fail, presents the failures to the developer; does not paper over

## Hard rules

- **Never skip a test.** User global rule. If a test is broken, fix it; if it's wrong, run feedback with the developer; never delete or skip silently.
- **Pure systems must be tested.** Profile/project may declare which systems are pure; those need unit tests.
- **Don't mock what you can integration-test.** Mocks should be a last resort.

For full instructions read `.ai-workflow/flows/test-flow.md`.
