---
name: docs-flow
description: Use when the developer says "update docs", "document X", "docs for X", "audit documentation", "fix the docs", or "knowledge base maintenance". Audits and updates the project knowledge base under flow-storage/{project,team,tasks}/ in 2 phases with 2 review gates [A]/[F]/[R] (one after diffs are applied, one before commit), including ARCHITECTURE.md, CONVENTIONS.md, PATTERNS.md, DECISIONS.md, lessons-learned.md, onboarding.md. Living documents only — never creates archived/versioned copies (Rule 2). The full flow is in .ai-workflow/flows/docs-flow.md.
license: MIT
metadata:
  audience: developers
  workflow: task-development
  phase: docs
---

# docs-flow (OpenCode wrapper)

This skill maintains the project knowledge base. Full flow at `.ai-workflow/flows/docs-flow.md`.

## When to invoke

- "Update docs"
- "Document {task-name}"
- "Audit the documentation"
- "Update PATTERNS.md after this PR"
- "Add the new ADR to DECISIONS.md"
- "Refresh onboarding.md"

## What this flow does

**2 phases, 2 gates.** Each phase contains multiple sub-tasks that auto-proceed without intermediate gates. See `.ai-workflow/universal/workflow-structure.md` "Phase Architecture".

### Phase 1: APPLY *(GATE TYPE B — STANDARD)*

Sub-tasks (auto-proceed):
- **1.1 SCAN** — Identify stale/missing/broken docs; build audit report.
- **1.2 PROPOSE** — Draft changes as diffs; cross-check against living-document policy (Rule 2).
- **1.3 APPLY** — Modify docs in place — **never** creates `docs-v2/` or `*-old.md`. Cross-check references (broken links, stale section names). If a task just merged, propagate lessons-learned and patterns into project-level docs.

→ **Gate 1: Doc updates review** — developer sees audit + applied diffs together.

### Phase 2: COMMIT *(GATE TYPE C — CRITICAL)*

Sub-tasks (auto-proceed):
- **2.1 COMMIT** *(executes only after the gate is accepted)* — Stage docs touched; compose `docs: update {scope} — {summary}`; `git commit`. Without this commit, doc edits sit uncommitted and silently revert on the next branch switch. Opt-out via `--no-commit` if bundling with code changes that come later.

→ **Gate 2: Commit review** — presented BEFORE 2.1 executes git commit.

## Hard rules

- **Living documents** (Rule 2) — update in place, never archive.
- **task-design.md is immutable after sign-off** (Rule 9) — deviations go in task-technical-design.md, this flow never edits a signed-off task-design.md.

For full instructions read `.ai-workflow/flows/docs-flow.md`.
