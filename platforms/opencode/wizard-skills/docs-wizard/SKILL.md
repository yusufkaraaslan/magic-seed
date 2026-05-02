---
name: docs-wizard
description: Use when the developer says "update docs", "document X", "docs for X", "audit documentation", "fix the docs", or "knowledge base maintenance". Audits and updates the project knowledge base under docs/{project,team,features}/, including ARCHITECTURE.md, CONVENTIONS.md, PATTERNS.md, DECISIONS.md, lessons-learned.md, onboarding.md. Living documents only — never creates archived/versioned copies (Rule 2). The full flow is in .ai-workflow/wizards/docs-wizard.md.
license: MIT
metadata:
  audience: developers
  workflow: feature-development
  phase: docs
---

# docs-wizard (OpenCode wrapper)

This skill maintains the project knowledge base. Full flow at `.ai-workflow/wizards/docs-wizard.md`.

## When to invoke

- "Update docs"
- "Document {feature}"
- "Audit the documentation"
- "Update PATTERNS.md after this PR"
- "Add the new ADR to DECISIONS.md"
- "Refresh onboarding.md"

## What this wizard does

1. Identifies which doc(s) need updating based on recent changes
2. Updates in-place per Rule 2 — **never** creates `docs-v2/` or `*-old.md`
3. Cross-checks references (broken links, stale section names)
4. If a feature just merged, propagates lessons-learned and patterns into project-level docs

## Hard rules

- **Living documents** (Rule 2) — update in place, never archive.
- **DESIGN.md is immutable after sign-off** (Rule 9) — deviations go in TDD.md, this wizard never edits a signed-off DESIGN.md.

## Final phase commits

The wizard's last phase is a CRITICAL-gate commit (`docs: update {scope} — {summary}`). Without this, doc edits sit uncommitted and silently revert on the next branch switch. Opt-out via `--no-commit` if bundling with code changes that come later.

For full instructions read `.ai-workflow/wizards/docs-wizard.md`.
