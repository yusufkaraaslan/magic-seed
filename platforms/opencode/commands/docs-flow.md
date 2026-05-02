---
description: Audit and update the project knowledge base — living documents only, no archives
agent: build
---

Run the **docs-flow** for: $ARGUMENTS

The argument is one of:
- `<scope>` — limit to a topic (e.g. `architecture`, `onboarding`, a specific task)
- (empty) — full audit of `flow-storage/project/`, `flow-storage/team/`, and `flow-storage/tasks/*/lessons-learned.md`

Load the `docs-flow` skill (or read `.ai-workflow/flows/docs-flow.md` directly) and execute its **2 phases**. Each phase contains sub-tasks that auto-proceed without intermediate gates:

### Phase 1: APPLY *(STANDARD gate)*
- **1.1 SCAN** — Identify stale/missing/broken docs by comparing last-modified vs recent commits, merged PRs, new tasks. Build an audit list with rationale.
- **1.2 PROPOSE** — Draft each change as a diff. Cross-check against living-document policy.
- **1.3 APPLY** — Apply diffs. Verify no broken cross-references.
- → **Gate 1: Doc updates review** — audit + applied diffs together.

### Phase 2: COMMIT *(CRITICAL gate)*
- **2.1 COMMIT** *(executes only after the gate is accepted)* — Stage updated docs (and any AGENTS.md / CLAUDE.md edits if those were in scope). Commit message: `docs: update {scope} — {summary}`. Skip 2.1 if `--no-commit`.
- → **Gate 2: Commit review** — presented BEFORE 2.1 executes git commit.

Hard rules:
- **Living documents** (Rule 2) — update in place. Never create `docs-v2/`, `*-old.md`, or dated copies.
- **task-design.md is immutable** (Rule 9) — deviations go in task-technical-design.md. This flow never edits a signed-off task-design.md.
- **Cross-references must stay valid** — if you rename or restructure, update every reference.

If $ARGUMENTS is empty, run a full audit and report the proposed update list before doing any APPLY work.
