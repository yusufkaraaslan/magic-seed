---
name: test-wizard
description: Use when the developer says "test X", "run tests", "add tests for X", "test the feature", "write tests", or "fix failing tests". Adds and runs tests using the project's test framework (GUT for Godot, pytest for Python, jest/vitest for JS, etc.). Enforces the user's global rule "never skip any test". The full flow is in .ai-workflow/wizards/test-wizard.md.
license: MIT
metadata:
  audience: developers
  workflow: feature-development
  phase: test
---

# test-wizard (OpenCode wrapper)

This skill adds and runs tests for a feature. Full flow at `.ai-workflow/wizards/test-wizard.md`.

## When to invoke

- "Test {feature}"
- "Add tests for {feature}"
- "Run the tests"
- "Write tests for {file}"
- "Fix failing tests"

## What this wizard does

1. Identifies the project's test framework (from profile and project config)
2. Locates the existing tests for the feature/files in question
3. Adds missing test coverage per project Rule 13 (test coverage for new features)
4. Runs the full test suite — **never skips, never partial-runs**
5. If tests fail, presents the failures to the developer; does not paper over

## Hard rules

- **Never skip a test.** User global rule. If a test is broken, fix it; if it's wrong, run feedback with the developer; never delete or skip silently.
- **Pure systems must be tested.** Profile/project may declare which systems are pure; those need unit tests.
- **Don't mock what you can integration-test.** Mocks should be a last resort.

For full instructions read `.ai-workflow/wizards/test-wizard.md`.
