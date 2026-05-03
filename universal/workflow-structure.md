# Workflow Structure

> **Applies to:** ALL profiles  
> **Purpose:** Defines the wizard architecture, phase patterns, and execution model

---

## Wizard Types

Every profile provides one or more wizards. The core set is:

| Wizard | Purpose | Input | Output |
|--------|---------|-------|--------|
| **design-wizard** | Design features with diagrams | Feature idea | DESIGN.md, TDD.md, EDGE-CASES.md, diagrams, issues |
| **implement-wizard** | Implement issues with validation | Issue files | Source code, updated issues (knowledge records) |
| **pr-wizard** | PR lifecycle management | Completed issues | Validation report, feedback issues, lessons learned |
| **test-wizard** | Test planning & execution | Feature docs | Test files, coverage report |
| **deploy-wizard** | Deployment & release | Build artifacts | Deployed release, verification report |
| **docs-wizard** | Documentation maintenance | Existing docs | Updated docs, new patterns catalog |

Profiles may add custom wizard types (e.g., `security-wizard`, `perf-wizard`).

---

## Phase Architecture

Every wizard is composed of **phases**. Phases are sequential, each ending with a review gate.

### Design Wizard Phases

```
Phase 1: UNDERSTAND
  → Load project context
  → Read existing docs
  → Explore codebase
  → Discover related features
  [GATE: Developer confirms understanding]

Phase 2: DESIGN (Visual-First)
  → Generate rough class diagram from developer description
  → Present diagram, iterate with developer
  → Once approved, generate text documentation
  → Create DESIGN.md
  [GATE: DESIGN.md review — [A]/[F]/[R]]

Phase 3: SPECIFY
  → Generate TDD.md from class diagram
  → Generate EDGE-CASES.md
  → Generate sequence/state diagrams for complex flows
  → Generate issue files (dynamically named based on feature analysis)
  [GATE: Specification review — [A]/[F]/[R]]

Phase 4: REVIEW
  → Present complete design package
  → Show diagrams alongside text
  → Developer reviews all artifacts
  [GATE: Final design approval — [A]/[F]/[R]]

Phase 5: PLAN
  → Create issue dependency diagram
  → Lock DESIGN.md (immutable)
  → Hand off to implement-wizard
```

### Implement Wizard Phases

```
Phase 1: READ
  → Load architecture context
  → Read issue file (acceptance criteria, files to create)
  → Read design docs (relevant sections)
  → Read completed prior implementations
  → Find reference implementations in codebase
  [GATE: Confirm context loaded]

Phase 2: PLAN
  → Analyze requirements
  → Map to existing patterns
  → Structure implementation
  → Create implementation plan document
  [GATE: Plan review — [A]/[F]/[R]]

Phase 3: IMPLEMENT
  → Write code incrementally
  → Follow profile conventions
  → Apply universal rules
  → Cite reference implementations
  [GATE: Implementation review — [A]/[F]/[R]]

Phase 4: AUTO-VALIDATE
  → Run architecture checks
  → Run style checks
  → Run tests
  → Check integration points
  [GATE: Validation report — [A]/[F]/[R]]

Phase 5: DOC-SYNC
  → Update TDD.md with deviations
  → Update EDGE-CASES.md with discoveries
  → Update PATTERNS.md with new patterns
  [GATE: Doc update review — [A]/[F]/[R]]

Phase 6: DEVELOPER REVIEW
  → Present complete implementation
  → Show files changed, tests run, coverage
  → Developer reviews
  [GATE: [A]ccept / [F]eedback / [R]eject]

Phase 7: UPDATE ISSUE
  → Transform issue to knowledge record
  → Add implementation notes
  → Record deviations and decisions
  → Set status: complete, accepted-date

Phase 8: COMMIT
  → Commit per issue
  → Write descriptive commit message
  → Reference issue number
```

### PR Wizard Modes

```
Mode: pre-pr (default)
  Step 1: Issue completeness check
  Step 2: Documentation check
  Step 3: Code quality validation
  Step 4: Cross-issue integration check
  Step 5: Test execution
  Step 6: Registration/integration check
  Step 7: Report PASS/FAIL

Mode: feedback
  Step 1: Initialize session
  Step 2: Per-comment loop
    → Developer pastes PR comment
    → AI analyzes (file, line, category, severity)
    → AI creates feedback issue file
    → AI suggests fix
    → Developer: [Y] apply / [N] skip / [M] manual
    → Update issue to knowledge record
  Step 3: Summary + commit

Mode: capture
  Step 1: Read all issue files
  Step 2: Generate lessons learned
  Step 3: Propose knowledge base updates
  Step 4: Update feature catalog
  Step 5: Update living documents
  Step 6: Capture metrics
  Step 7: Completion summary
```

---

## Phase Gate Protocol

Every phase ends with the same review protocol:

### Present

Show the developer:
- What was done in this phase
- Key decisions made
- Files created/modified
- Any warnings or issues

### Ask

```
Phase {N}: {Name} complete.

Summary:
  - {key accomplishment 1}
  - {key accomplishment 2}
  - {key accomplishment 3}

Review and choose:
  [A]ccept — Proceed to next phase
  [F]eedback — Provide feedback, I'll revise
  [R]eject — Start this phase over

Your choice:
```

### Handle Feedback

If **Feedback**:
1. Developer provides specific feedback
2. AI revises based on feedback
3. Re-present with changes highlighted
4. Loop until Accept or Reject

If **Reject**:
1. Ask developer: "Start over from beginning of phase, or specific sub-step?"
2. Restart from specified point
3. Previous work is not lost (saved in temp state)

If **Accept**:
1. Mark phase complete
2. Save artifacts
3. Proceed to next phase

---

## Resume Logic

Wizards support resuming across sessions.

**Detection:**
- Check issue frontmatter for `status`
- Read all issue files in `docs/features/{name}/issues/`
- Find first issue with `status: pending` or `status: in_progress`

**Resume message:**
```
Implementation Status: {feature-name}

  model.md       complete   (2026-04-15)
  controller.md  in_progress
  view.md        pending    (blocked by: controller)
  tests.md       pending    (blocked by: model, controller)
  integration.md pending    (blocked by: all above)

Progress: 1/5 complete

Resuming: controller.md
```

---

## Status Mode

Every wizard supports `--status` flag:

```
/wizard-name {feature} --status
```

**Output:**
- Read all relevant files without modifying
- Show progress summary
- Identify blockers
- Suggest next action

**No implementation in status mode.** Read-only.

---

## Custom Wizard Types

Profiles and users can define custom wizard types.

**Requirements for a new wizard:**
1. Markdown file in `.ai-workflow/wizards/` or profile `skeletons/`
2. Clear purpose statement
3. Defined phases with gates
4. Specified input/output artifacts
5. Review protocol at each gate

**Example custom wizard:**
```markdown
# Wizard: Security Audit

## Purpose
Run security checks on new or modified features.

## When to Use
After implement-wizard, before PR.

## Phases

### Phase 1: SCAN
- Search for SQL injection risks
- Check for exposed secrets
- Validate input sanitization
- Check auth middleware coverage
[GATE: Present findings]

### Phase 2: ASSESS
- Categorize findings by severity
- Map to OWASP Top 10
- Check against project security requirements
[GATE: Present assessment]

### Phase 3: REPORT
- Generate security report
- Suggest fixes with code examples
- Create security issue files if needed
[GATE: [A]/[F]/[R]]
```

---

## Artifact Flow

```
Developer idea
    ↓
/design-wizard {name}
    ↓
DESIGN.md + diagrams (class, package, sequence)
    ↓
TDD.md + EDGE-CASES.md
    ↓
Issue files (model, controller, view, tests, integration)
    ↓
/implement-wizard {name} --issue=model
    ↓
Model implementation + tests
    ↓
Knowledge record (model.md updated)
    ↓
/implement-wizard {name} --issue=controller
    ↓
Controller implementation + tests
    ↓
Knowledge record (controller.md updated)
    ↓
... (repeat for all issues)
    ↓
/pr-wizard {name} --mode=pre-pr
    ↓
Validation report (PASS/FAIL)
    ↓
Developer creates PR
    ↓
Reviewers comment
    ↓
/pr-wizard {name} --mode=feedback
    ↓
Feedback issues → fixes → knowledge records
    ↓
PR merged
    ↓
/pr-wizard {name} --mode=capture
    ↓
lessons-learned.md + PATTERNS.md updates
    ↓
Feature complete
```

---

## Extending the Workflow

### Add a phase to existing wizard

Edit the wizard markdown file, add a new phase section following the same pattern.

### Add a new wizard type

Create new markdown file in `.ai-workflow/wizards/` with phases, gates, and artifacts.

### Modify review options

Change the gate prompt text, but keep the [A]/[F]/[R] structure.

### Add artifacts

Specify new output files in phase descriptions. Update knowledge-base-spec.md if needed.
