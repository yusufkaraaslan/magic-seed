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

## Mode: pre-pr

### Step 1: Issue Completeness

Check all task flows:
- Status must be `complete`
- Must have `accepted-date`

**If incomplete:**
```
Cannot proceed: N issue(s) not complete

  02-logic.md  in_progress
  03-integration.md  pending

Run the implement flow for {name} to complete.
```

### Step 2: Documentation Check

Verify:
- [ ] task-design.md exists and is signed off
- [ ] task-technical-design.md exists
- [ ] task-edge-cases.md exists
- [ ] No `{placeholder}` text in final docs
- [ ] All diagrams are present

### Step 3: Code Quality

Check:
- [ ] Naming conventions followed
- [ ] Documentation present
- [ ] No TODO/FIXME left (or documented)
- [ ] No secrets/hardcoded values
- [ ] Error handling present

### Step 4: Test Validation

If test framework available:
- [ ] All new tests pass
- [ ] Existing tests still pass (no regressions)
- [ ] Coverage meets minimum (if specified)

### Step 5: Integration Check

Verify:
- [ ] New code integrates with existing code
- [ ] No breaking changes (or documented)
- [ ] Dependencies are satisfied

### Step 6: Report

Generate validation report:
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

**Gate:** [A]ccept / [F]ix issues / [R]eject

---

## Mode: feedback

### Step 1: Initialize Session

1. Create/load `FEEDBACK-LOG.md`
2. Count existing feedback task flows
3. Prompt developer:
```
PR Feedback Handler: {task-name}

Paste PR comments one at a time.
Type 'done' when finished.

Paste comment:
>
```

### Step 2: Per-Comment Loop

For each comment:

1. **Developer pastes comment**

2. **AI analyzes:**
   - File/line referenced
   - Category: architecture / style / logic / performance / security / docs
   - Severity: blocking / suggestion / nitpick
   - Type: bug / improvement / question / praise

3. **AI creates feedback issue:**
   ```markdown
   ---
   issue: feedback-{N}
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
   - Category: {category}
   - Severity: {severity}
   
   ## Fix
   {suggested fix with before/after}
   
   ## Apply?
   [Y]es / [N]o / [M]anual
   ```

4. **Developer chooses:**
   - **[Y]es** — AI applies fix
   - **[N]o** — Skip this comment
   - **[M]anual** — Developer handles manually

5. **If Yes:**
   - Apply fix
   - Update feedback issue with fix details
   - Set status: resolved

6. **Loop** until developer types "done"

### Step 3: Summary

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

Next: Commit fixes and push
```

---

## Mode: capture

### Step 1: Read All Issues

Read every issue file:
- Implementation task flows (model, logic, integration, tests, docs)
- Feedback task flows (feedback-1, feedback-2, ...)

Extract:
- Implementation patterns
- Deviations from design
- Feedback themes
- Time spent
- Iterations

### Step 2: Generate Lessons Learned

Create `flow-storage/tasks/{task-name}/lessons-learned.md`:

```markdown
# Lessons Learned: {task-name}

## What Went Well
- {positive 1}
- {positive 2}

## What Was Missed
- {missed 1}
- {missed 2}

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

### Step 3: Propose KB Updates

Suggest updates to:
- `flow-storage/project/PATTERNS.md` — Add new patterns
- `flow-storage/project/DECISIONS.md` — Add ADRs for major changes
- `flow-storage/project/CONVENTIONS.md` — Update if conventions evolved

Developer approves each update: [Y]es / [N]o

### Step 4: Update Living Documents

- task-design.md — Add post-merge notes (not changing signed-off content)
- task-technical-design.md — Add final deviations
- task-edge-cases.md — Add new edge cases from PR

### Step 5: Complete

```
Knowledge Capture Complete: {task-name}

Updated:
  ✓ lessons-learned.md
  ✓ PATTERNS.md (2 new patterns)
  ✓ CONVENTIONS.md (1 update)

Task complete and knowledge captured!
```

---

## Customization

Users can customize:
- Add validation checks in pre-pr mode
- Change feedback categories
- Add metrics to capture
- Add team notification after capture
