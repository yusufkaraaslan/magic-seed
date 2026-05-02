---
description: Audit and update the project knowledge base — living documents only, no archives
agent: build
---

Run the **docs-wizard** for: $ARGUMENTS

The argument is one of:
- `<scope>` — limit to a topic (e.g. `architecture`, `onboarding`, a specific feature)
- (empty) — full audit of `docs/project/`, `docs/team/`, and `docs/features/*/lessons-learned.md`

Load the `docs-wizard` skill (or read `.ai-workflow/wizards/docs-wizard.md` directly) and execute:

1. **SCAN** *(LIGHT)* — Identify stale or missing docs by comparing last-modified vs recent commits, merged PRs, new features. Build a list with rationale.
2. **PROPOSE** *(STANDARD)* — Draft each change as a diff. Cross-check against living-document policy.
3. **APPLY** *(STANDARD)* — Apply accepted diffs. Verify no broken cross-references.
4. **COMMIT** *(CRITICAL)* — Stage updated docs (and any AGENTS.md / CLAUDE.md edits if those were in scope). Commit message: `docs: update {scope} — {summary}`. Skip if `--no-commit`.

Hard rules:
- **Living documents** (Rule 2) — update in place. Never create `docs-v2/`, `*-old.md`, or dated copies.
- **DESIGN.md is immutable** (Rule 9) — deviations go in TDD.md. This wizard never edits a signed-off DESIGN.md.
- **Cross-references must stay valid** — if you rename or restructure, update every reference.

If $ARGUMENTS is empty, run a full audit and report the proposed update list before doing any APPLY work.
