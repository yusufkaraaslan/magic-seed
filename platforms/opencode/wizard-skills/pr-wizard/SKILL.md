---
name: pr-wizard
description: Use when the developer says "validate X for PR", "PR check", "ready for PR", "pre-PR review", "open PR for X", or "is X ready to merge". Pre-PR validation checklist — runs full test suite, verifies type hints, signal cleanup, no leaked TODOs, architecture compliance, and naming conventions. Also handles post-merge knowledge capture (lessons-learned, patterns) when developer says "capture lessons" or "post-merge cleanup". The full flow is in .ai-workflow/wizards/pr-wizard.md.
license: MIT
metadata:
  audience: developers
  workflow: feature-development
  phase: pr
---

# pr-wizard (OpenCode wrapper)

This skill runs the **PR validation** flow plus PR feedback handling and post-merge knowledge capture. Three modes: pre-pr, feedback, capture. Full flow at `.ai-workflow/wizards/pr-wizard.md`.

## When to invoke

- "Validate {feature} for PR"
- "PR check for {feature}"
- "Is {feature} ready to merge?"
- "Process PR feedback for {feature}"
- "Capture lessons learned for {feature}"

## Pre-PR mode runs the full checklist

Pulls from `.ai-workflow/universal/rules.md`, the profile's `rules.md`, and `.ai-workflow/rules.md`. Verifies:

- All issues for the feature complete
- DESIGN.md signed off
- Tests pass (project's test command — never skip)
- No leaked `{slot}` placeholders (Rule 12)
- Naming conventions
- No architectural violations (project-specific rules)
- TDD.md and EDGE-CASES.md updated

## Hard rule

Never present a PR as "ready" with failing tests. The user's global rule "never skip any test" applies.

For full instructions read `.ai-workflow/wizards/pr-wizard.md`.
