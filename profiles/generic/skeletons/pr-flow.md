# Flow: PR

> **Profile:** Generic  
> **Purpose:** PR lifecycle — validate, handle feedback, capture knowledge

---

## When to Use

After the implement flow has completed all task flows.

## Modes

| Mode | How to invoke | When to Use |
|------|---------------|-------------|
| **pre-pr** | Run the PR flow for `{name}` (default mode) | Before creating PR |
| **feedback** | Run the PR flow for `{name}` in `feedback` mode | PR has review comments |
| **capture** | Run the PR flow for `{name}` in `capture` mode | After PR is merged |

## Input

- All task flow files from `flow-storage/tasks/{task-name}/implement/flow-plan/`
- task-design.md, task-technical-design.md
- Source code changes

## Output

- Validation report (pre-pr)
- Feedback task flows (feedback)
- Lessons learned + KB updates (capture)

---

Each mode has **2 phases, 2 gates** (Phase 1 = work, Phase 2 = commit/submit). Sub-tasks within each phase auto-proceed. See `universal/workflow-structure.md`.

---

## Mode: pre-pr

### Phase 1: VALIDATE

**Purpose:** Run all PR-readiness checks and produce a single report.

**Sub-tasks (auto-proceed):**

**1.1 Task flow completeness**
- Status must be `complete` for all task flows
- Must have `accepted-date`
- If incomplete, abort with the missing list — point developer at implement-flow

**1.2 Documentation check**
- [ ] `task-design.md` exists and is signed off
- [ ] `task-technical-design.md` exists
- [ ] `task-edge-cases.md` exists
- [ ] No `{placeholder}` text in final docs
- [ ] All diagrams are present and rendered

**1.3 Code quality**
- [ ] Naming conventions followed
- [ ] Documentation present
- [ ] No TODO/FIXME left (or documented)
- [ ] No secrets/hardcoded values
- [ ] Error handling present

**1.4 Test validation**
- [ ] All new tests pass
- [ ] Existing tests still pass (no regressions)
- [ ] Coverage meets minimum (if specified)

**1.5 Integration check**
- [ ] New code integrates with existing code
- [ ] No breaking changes (or documented)
- [ ] Dependencies satisfied

**Artifacts (presented at gate):**
```
Pre-PR Validation: {task-name}

Documentation:     ✓ PASS
Code Quality:      ✓ PASS
Tests:             ✓ PASS (12/12)
Integration:       ⚠ WARNING (1 issue)

Issues Found:
  1. [WARNING] No tests for edge case EC3
     Suggestion: Add test for empty input

Status: READY FOR PR (with minor fixes)
```

**Gate:** Validation review — [A]ccept / [F]eedback / [R]eject  
*(GATE TYPE B — STANDARD)*

---

### Phase 2: REPORT

**Purpose:** Generate the PR description and open the PR (or print it for review).

**Sub-tasks (auto-proceed):**

**2.1 Generate PR description** from task flows + commits + diagrams (link to `task-design.md`)

**2.2 OPEN PR** *(executes only after Phase 2 gate is accepted)*
- Open the PR with the generated description
- (If `--dry-run`, print the description and skip opening)

**Artifacts (presented at gate):**
- PR description draft
- Branch state (up to date with target?)
- Target branch + reviewers (if known)

**Gate:** PR submission review — [A]ccept / [F]eedback / [R]eject  
*(GATE TYPE C — CRITICAL)*

⚠️ Once accepted, the PR is publicly visible to reviewers.

**Failure modes to verify before [A]ccept:**
- All Phase 1 validation checks PASS (or [W]arnings explicitly accepted)?
- PR description captures the *why* and links task flows?
- Branch up to date with target?

**Opt-out:** `--dry-run` skips sub-task 2.2 (gate still presents).

---

## Mode: feedback

### Phase 1: PROCESS

**Purpose:** Walk through each PR comment, propose fixes, and apply approved ones — all as one reviewable batch.

**Sub-tasks (auto-proceed):**

**1.1 Initialize session**
- Create/load `FEEDBACK-LOG.md`
- Count existing feedback task flows

**1.2 Per-comment loop** (for each comment the developer pastes)
- AI analyzes: file/line, category (architecture/style/logic/performance/security/docs), severity (blocking/suggestion/nitpick), type (bug/improvement/question/praise)
- AI creates a feedback task flow file:
  ```markdown
  ---
  task-flow: feedback-{N}
  task: "{task-name}"
  reviewer: "{name}"
  category: "{category}"
  severity: "{severity}"
  status: open
  ---

  ## Original Comment
  {comment text}

  ## Analysis
  - File: {file}
  - Line: {line}

  ## Fix
  {suggested fix with before/after}

  ## Apply?
  [Y]es / [N]o / [M]anual
  ```
- Developer chooses:
  - **[Y]es** — AI applies fix; status: resolved
  - **[N]o** — skip
  - **[M]anual** — developer handles
- Loop until developer types "done"

**Artifacts (presented at gate):**
```
Feedback Summary: {task-name}

Total comments: 5
Applied: 3
Skipped: 1
Manual: 1

Categories:
  style: 2
  logic: 2
  docs: 1
```
- Feedback task flow files (one per comment)
- Code fixes applied (file diffs)

**Gate:** Feedback review — [A]ccept / [F]eedback / [R]eject  
*(GATE TYPE B — STANDARD)*

---

### Phase 2: COMMIT

**Purpose:** Commit feedback task flows + applied fixes as one unit.

**Sub-tasks (auto-proceed):**

**2.1 COMMIT** *(executes only after Phase 2 gate is accepted)*
- Stage feedback task flow files + any code fixes applied
- Compose commit message:
  ```
  fix({task-name}): apply PR feedback ({N} comments)

  - {fix 1}
  - {fix 2}
  ```
- Run `git commit`

**Artifacts (presented at gate):**
- Staged file list (`git diff --cached --stat`)
- Proposed commit message

**Gate:** Commit review — [A]ccept / [F]eedback / [R]eject  
*(GATE TYPE C — CRITICAL)*

**Failure modes to verify before [A]ccept:**
- All feedback task flows have a fix / skip / manual decision recorded?
- Code fixes correspond 1:1 to feedback task flows marked `[Y]es`?

**Opt-out:** `--no-commit` skips sub-task 2.1.

---

## Mode: capture

### Phase 1: ANALYZE

**Purpose:** Read all task work, generate lessons, propose KB updates, and apply them as one reviewable package.

**Sub-tasks (auto-proceed):**

**1.1 Read all task flow files**
- Implementation task flows (model, logic, integration, tests, docs)
- Feedback task flows (feedback-1, feedback-2, ...)
- Extract: patterns, deviations, feedback themes, time spent, iterations

**1.2 Generate lessons learned**
Create `flow-storage/tasks/{task-name}/docs/lessons-learned.md`:
```markdown
# Lessons Learned: {task-name}

## What Went Well
- {positive 1}

## What Was Missed
- {missed 1}

## New Patterns
- {pattern 1 with code example}

## Process Improvements
- {improvement 1}

## Metrics
- Design iterations: N
- Implementation task flows: N
- PR feedback comments: N
- Time from design to merge: N days
```

**1.3 Propose KB updates**
- `flow-storage/project/PATTERNS.md` — add new patterns
- `flow-storage/project/DECISIONS.md` — add ADRs for major changes
- `flow-storage/project/CONVENTIONS.md` — update if conventions evolved

**1.4 Update task catalog** (if profile uses one)

**1.5 Update living documents**
- `task-technical-design.md` — final deviations
- `task-edge-cases.md` — new edge cases from PR

**1.6 Capture metrics**

**Artifacts (presented at gate):**
- `lessons-learned.md` draft
- Proposed PATTERNS.md / DECISIONS.md / CONVENTIONS.md diffs
- Metrics summary

**Gate:** Capture review — [A]ccept / [F]eedback / [R]eject  
*(GATE TYPE B — STANDARD)*

---

### Phase 2: COMMIT

**Purpose:** Commit all knowledge artifacts as one unit.

**Sub-tasks (auto-proceed):**

**2.1 COMMIT** *(executes only after Phase 2 gate is accepted)*
- Stage updated `flow-storage/project/PATTERNS.md`, `flow-storage/project/DECISIONS.md`, `flow-storage/tasks/{task-name}/docs/lessons-learned.md`, and any other living documents touched
- Compose commit message:
  ```
  docs(capture): {task-name} lessons learned and patterns

  Patterns added: {list}
  ADRs added: {list}
  ```
- Run `git commit`

**Artifacts (presented at gate):**
- Staged file list (`git diff --cached --stat`)
- Proposed commit message

**Gate:** Commit review — [A]ccept / [F]eedback / [R]eject  
*(GATE TYPE C — CRITICAL)*

**Failure modes to verify before [A]ccept:**
- Files staged: only docs touched by capture — no stray edits to task code or `task-design.md` (immutable per Rule 9)
- PATTERNS.md/DECISIONS.md edits use established sections
- Commit message lists every pattern/ADR added

**Opt-out:** `--no-commit` skips sub-task 2.1; capture artifacts stay uncommitted.

---

## Customization

Users can customize:
- Add validation checks in pre-pr mode
- Change feedback categories
- Add metrics to capture
- Add team notification after capture
