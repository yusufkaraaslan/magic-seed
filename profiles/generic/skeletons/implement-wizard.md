# Wizard: Implement

> **Profile:** Generic  
> **Purpose:** Implement issues one at a time with validation and review

---

## When to Use

After the design wizard has completed and design is signed off.

## Input

- Issue files from `docs/features/{name}/issues/`
- DESIGN.md and TDD.md
- Prior completed issues (for context)

## Output

- Implemented source code
- Updated issue files (transformed to knowledge records)
- Tests
- Updated TDD.md with deviations

## Prerequisites

- Design wizard completed and signed off
- At least one issue file exists

---

## Phases

### Phase 1: READ

**Purpose:** Gather all context for this specific issue.

**Steps:**
1. Read project context:
   - `docs/project/ARCHITECTURE.md`
   - `docs/project/CONVENTIONS.md`
   - `docs/project/PATTERNS.md`
2. Read the issue file:
   - Extract acceptance criteria
   - Extract files to create
   - Extract dependencies
3. Read design docs:
   - `DESIGN.md` (relevant sections)
   - `TDD.md` (relevant sections)
   - `EDGE-CASES.md` (relevant cases)
4. Read completed prior issues:
   - Check which issues are `status: complete`
   - Read their implementation notes
5. Find reference implementations:
   - Search for similar features in codebase
   - Read 1-2 examples

**Artifacts:**
- Context summary

**Gate:** Confirm context loaded — [A]ccept / [M]odify scope / [R]eject

---

### Phase 2: PLAN

**Purpose:** Analyze requirements and plan implementation.

**Steps:**
1. Analyze each acceptance criterion:
   - What code is needed?
   - What patterns apply?
   - What edge cases exist?
2. Map to existing patterns:
   - Check `PATTERNS.md` for reusable patterns
   - Check reference implementations
3. Structure the implementation:
   - Files to create/modify
   - Order of implementation
   - Test approach
4. Create implementation plan document (temporary)

**Artifacts:**
- Implementation plan

**Gate:** Plan review — [A]ccept / [F]eedback / [R]eject

---

### Phase 3: IMPLEMENT

**Purpose:** Write the code.

**Steps:**
1. Create files incrementally:
   - Start with domain/model layer
   - Add business logic
   - Add integration/API layer
2. Follow conventions:
   - Naming from `CONVENTIONS.md`
   - Patterns from `PATTERNS.md`
   - Architecture from `ARCHITECTURE.md`
3. Handle edge cases from `EDGE-CASES.md`
4. Add documentation/comments
5. Cite reference implementations

**Artifacts:**
- Source code files
- Updated issue file (in_progress)

**Gate:** Implementation review — [A]ccept / [F]eedback / [R]eject

---

### Phase 4: AUTO-VALIDATE

**Purpose:** Run automated checks.

**Steps:**
1. Check for syntax errors
2. Check naming conventions
3. Check for TODO/FIXME comments
4. Verify all acceptance criteria are addressed
5. Check for common issues:
   - Missing error handling
   - Hardcoded values
   - Duplicated code
6. Run tests if test framework is available

**Artifacts:**
- Validation report

**Gate:** Validation review — [A]ccept / [F]eedback / [R]eject

---

### Phase 5: DOC-SYNC

**Purpose:** Update living documentation.

**Steps:**
1. Update TDD.md with deviations:
   - What changed from original design
   - Why it changed
2. Update EDGE-CASES.md with new discoveries
3. Update PATTERNS.md if new pattern discovered
4. Add implementation notes to issue file

**Artifacts:**
- Updated TDD.md
- Updated EDGE-CASES.md
- Updated PATTERNS.md
- Updated issue file

**Gate:** Doc update review — [A]ccept / [F]eedback / [R]eject

---

### Phase 6: DEVELOPER REVIEW

**Purpose:** Present complete implementation for developer approval.

**Steps:**
1. Show files changed:
   ```
   Files Created:
     - {{source-dir}}/features/auth/UserService.{{ext}}
     - {{source-dir}}/features/auth/PasswordValidator.{{ext}}
   
   Files Modified:
     - {{source-dir}}/features/auth/index.{{ext}}
   ```
2. Show test results (if available)
3. Show validation report
4. Highlight key decisions

**Artifacts:**
- Complete implementation

**Gate:** [A]ccept / [F]eedback / [R]eject

---

### Phase 7: UPDATE ISSUE

**Purpose:** Transform issue to knowledge record.

**Steps:**
1. Update issue frontmatter:
   ```yaml
   status: complete
   accepted-date: 2026-04-23
   ```
2. Add implementation notes:
   - What was implemented
   - Patterns used
   - Deviations from design
   - Testing approach
3. Add code references:
   - File paths and line numbers
   - Key functions/classes

**Artifacts:**
- Knowledge record (updated issue file)

---

### Phase 8: COMMIT

**Purpose:** Commit the work.

**Steps:**
1. Stage changes
2. Write commit message:
   ```
   feat({feature-name}): {issue-type} — brief description
   
   - Change 1
   - Change 2
   
   Refs: #{issue-number}
   ```
3. Commit
4. Report completion

**Artifacts:**
- Committed code

---

## Resume Logic

Before starting, check for existing progress:

1. Read all issue files in `docs/features/{name}/issues/`
2. Find first issue with `status: pending` or `status: in_progress`
3. If found, ask: "Resume from {issue-name}?"
4. If yes, skip completed issues

**Status display:**
```
Implementation Status: {feature-name}

  01-user-session-store.md    complete   (2026-04-23)
  02-auth-service.md          in_progress
  03-login-form.md            pending    (blocked by: auth-service)
  04-api-integration.md       pending    (blocked by: auth-service)
  05-tests.md                 pending    (blocked by: all above)

Progress: 1/5 complete
Next: 02-auth-service.md
```

---

## Customization

Users can customize:
- Add validation steps in Phase 4
- Change commit message format in Phase 8
- Add deployment step after commit
- Add notification step (Slack, email)
