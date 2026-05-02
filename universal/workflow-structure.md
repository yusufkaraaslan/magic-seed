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
  → Load project context (universal/, profiles/{detected}/, docs/project/*)
  → Read existing docs and architecture
  → Explore codebase for related features
  [GATE TYPE A — LIGHT]
  Agent reports loaded files + key facts, auto-proceeds.

Phase 2: DESIGN (Visual-First)
  → Generate class diagram, package diagram, sequence diagrams as needed
  → Per universal/diagram-standards.md
  → Render every diagram source to .svg (or .png fallback) — both source
    and image are committed. An unrendered .puml is not a deliverable
    (Rule 4 — diagrams mandatory means *visible* diagrams).
  → If the local toolchain has no renderer (no `plantuml`, no Java+jar,
    no Docker), STOP the phase. Tell the developer the install command
    for their OS (see diagram-standards.md "Local Rendering"). Resume
    after install + render succeeds. Do not present the gate with
    unrendered diagrams.
  [GATE TYPE B — STANDARD]

Phase 3: SPECIFY
  → Write DESIGN.md, TDD.md, EDGE-CASES.md
  → Cross-reference diagrams from Phase 2
  [GATE TYPE B — STANDARD]

Phase 4: PLAN
  → Decompose feature into implementation issues
  → Each issue gets frontmatter (issue, feature, status, depends-on, accepted-date)
  → Each issue declares Files to Create / Files to Modify
  → Build issue dependency graph
  [GATE TYPE C — CRITICAL]
  Failure modes to verify before [A]ccept:
    - Temporal contradiction: an issue marked "Files to Modify" against a
      file no prior issue has "Files to Create". Reorder or merge.
    - Type/symbol references: every type, enum, or class named in a
      Files to Create / Files to Modify list must be defined either in
      this feature's design or already in the codebase. Hunt for
      undefined references (e.g. a parameter `foo: BarType` where
      BarType is mentioned nowhere).
    - Coverage: scan DESIGN.md sections — is every public method,
      signal, and resource covered by exactly one issue?
    - Independence: any issue listed as parallel-with another that
      actually shares a file? Mark dependent or merge.
    - Scope creep: any issue larger than a one-day implementation?
      Split it.

Phase 5: FINALIZE
  → Sign off DESIGN.md (status: v1.0 — Signed Off, Immutable: Yes)
  → Confirm all issue files written, dep graph diagram generated
  [GATE TYPE C — CRITICAL]
  ⚠️ Once accepted, DESIGN.md becomes IMMUTABLE per Rule 9.
  Failure modes to verify before [A]ccept:
    - Anything you'd want to change in DESIGN.md? Now is the only
      time. Post-sign-off changes go in TDD.md only, never DESIGN.md.
    - Diagrams cross-referenced correctly from DESIGN.md?
    - All diagrams rendered: every `.puml` (or `.d2` / `.mmd`) has a
      sibling `.svg` or `.png` in the same diagrams/ directory.
      Run: `for f in docs/features/{name}/diagrams/*.puml; do
              base="${f%.puml}"; [ -f "${base}.svg" ] || [ -f "${base}.png" ]
              || echo "MISSING IMAGE: $f"
            done`
      Any "MISSING IMAGE" output means render before signing off.
    - All issues from Phase 4 actually written to disk?
    - References section points to real Docs/* files?

Phase 6: COMMIT
  → Stage docs/features/{name}/ entirely (DESIGN.md, TDD.md,
    EDGE-CASES.md, diagrams/*.puml + *.svg/*.png, issues/*.md)
  → Compose commit message:
      design({feature-name}): sign off DESIGN.md and {N} issues
      
      {one-line summary of feature scope}
      
      Issues created: {issue list, comma-separated}
      Diagrams: class, package, sequence (and any others)
  → Run git commit
  [GATE TYPE C — CRITICAL]
  ⚠️ DESIGN.md "Immutable: Yes" claim is meaningful only if locked
  in git history. An uncommitted "immutable" file is editable by
  anyone — convention only.
  Failure modes to verify before [A]ccept:
    - Files staged: only docs/features/{name}/ contents — no stray
      edits from other features or from .ai-workflow/. Run
      `git diff --cached --stat` and verify the file list.
    - Commit message: feature name correct? Issue count matches
      what's in issues/? Summary captures the *why* not the *what*?
    - Hooks won't fail (no linter complaint, no missing files).
    - If --no-commit flag passed, skip this phase entirely (the
      developer wants to bundle multiple wizard runs into one commit).

  Opt-out: developer can pass --no-commit when invoking the wizard
  to skip Phase 6. Use when bundling design with adjacent work into
  a single commit (e.g., design + plane.so issue creation in one go).
```

### Implement Wizard Phases

```
Phase 1: READ
  → Load architecture context (rules, profile, project docs)
  → Read issue file (acceptance criteria, files to create/modify, deps)
  → Read DESIGN.md and TDD.md sections relevant to this issue
  → Find reference implementations in codebase
  [GATE TYPE A — LIGHT]
  Agent reports issue loaded + key acceptance criteria + reference files
  found, auto-proceeds. Hard refusal if no DESIGN.md or DESIGN.md not
  signed off — point developer at design-wizard.

Phase 2: PLAN
  → Analyze acceptance criteria against existing patterns
  → Structure the implementation (file order, function shape)
  → Surface ambiguities or design gaps
  [GATE TYPE B — STANDARD]

Phase 3: IMPLEMENT
  → Write code incrementally per profile conventions
  → Apply universal rules and project rules
  → Cite reference implementations in comments only when non-obvious
  [GATE TYPE B — STANDARD]

Phase 4: AUTO-VALIDATE
  → Run architecture/style checks specified in profile rules.md
  → Run the project's test suite (NEVER skip tests — user global rule)
  → Check integration points against DESIGN.md signal flow
  [GATE TYPE B — STANDARD]
  If validation fails, surface failures clearly and offer Fix/Defer/Skip
  per failure (Skip only with developer override).

Phase 5: DOC-SYNC
  → Update TDD.md with any deviations from DESIGN.md (Rule 9 — DESIGN
    stays immutable; deviations live in TDD)
  → Update EDGE-CASES.md with discoveries
  → Update docs/project/PATTERNS.md with new reusable patterns
  [GATE TYPE B — STANDARD]

Phase 6: DEVELOPER REVIEW
  → Present complete implementation: files changed, tests run, coverage
  [GATE TYPE B — STANDARD]

Phase 7: UPDATE ISSUE
  → Transform issue file to knowledge record (status: complete,
    accepted-date set, implementation notes appended)
  [GATE TYPE A — LIGHT]
  Agent reports issue file updates and proceeds.

Phase 8: COMMIT
  → Stage only the files this issue produced
  → Compose commit message (conventional commits: feat/fix/docs/test)
  → Reference issue number
  [GATE TYPE C — CRITICAL]
  ⚠️ Commit lands in history; amending later is friction.
  Failure modes to verify before [A]ccept:
    - Files staged: any unrelated stray edits sneaking in? Run
      `git diff --cached` mentally and verify each entry belongs.
    - Commit message: scope/type accurate? Description focuses on
      *why* not *what*?
    - Co-author trailer present if required by project policy?
    - Hooks won't fail (e.g. linter clean, no test failures)?
    - Pre-existing failing tests not silently bypassed?
```

### Docs Wizard Phases

```
Phase 1: SCAN
  → Identify which docs are stale or missing (compare last
    modified vs recent commits / merged PRs / new features)
  → Build a list of docs to update with rationale
  [GATE TYPE A — LIGHT]
  Agent reports the scan + proposed update list, auto-proceeds.

Phase 2: PROPOSE
  → For each doc to update, draft the change as a diff
  → Cross-check against living-document policy (Rule 2):
    update in place, no archives, no v2 copies
  [GATE TYPE B — STANDARD]

Phase 3: APPLY
  → Apply each accepted diff to its doc
  → Verify no broken cross-references introduced
  [GATE TYPE B — STANDARD]

Phase 4: COMMIT
  → Stage updated docs/ files (and any AGENTS.md / CLAUDE.md
    edits if those were part of scope)
  → Compose commit message:
      docs: update {scope} — {one-line summary}
      
      Files updated: {list}
  → Run git commit
  [GATE TYPE C — CRITICAL]
  Failure modes to verify before [A]ccept:
    - Files staged: only docs touched by this run — no code edits
      sneaking in (docs-wizard never touches code).
    - Living-document compliance: did the diffs update files in
      place, or did they create *-v2.md / archive copies? The
      latter violates Rule 2.
    - DESIGN.md untouched (Rule 9 — immutable).
  Opt-out: --no-commit to defer.
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
  Step 5: Update living documents (PATTERNS.md, DECISIONS.md,
          docs/features/{name}/lessons-learned.md)
  Step 6: Capture metrics
  Step 7: Completion summary
  Step 8: COMMIT [GATE TYPE C — CRITICAL]
    → Stage updated docs/project/PATTERNS.md, docs/project/DECISIONS.md,
      docs/features/{name}/lessons-learned.md, and any other living
      documents touched
    → Compose commit message:
        docs(capture): {feature} lessons learned and patterns
        
        Patterns added: {list}
        ADRs added: {list}
    → Run git commit
    Failure modes to verify before [A]ccept:
      - Files staged: only the docs touched by capture — no stray
        edits to feature code or DESIGN.md (which is immutable).
      - PATTERNS.md/DECISIONS.md edits use established sections
        (no random new sections that break the doc structure).
      - Commit message lists every pattern/ADR added.
    Opt-out: --no-commit to defer; capture artifacts stay uncommitted.
```

---

## Phase Gate Protocol

Not every phase needs the same level of review. A "I just read the project files" phase doesn't deserve the same ceremony as "I'm about to lock DESIGN.md immutable forever." Three gate types exist; each wizard phase declares which one it uses.

### Gate type A — LIGHT (auto-proceed with summary)

Use for **context-loading** phases that don't produce reviewable artifacts (Phase 1 UNDERSTAND, Phase 1 READ in implement-wizard, etc.). The agent reports what it loaded and proceeds without asking. The developer can interrupt at any time, but no blocking gate. Saves a click per wizard run for a phase where the answer is almost always "yes."

```
Phase {N}: {Name} complete.

Loaded:
  - {file/source 1}
  - {file/source 2}
  - {file/source 3}

Key facts found:
  - {fact 1}
  - {fact 2}

Proceeding to Phase {N+1}: {Next Name}.
(Interrupt now if anything above is wrong.)
```

### Gate type B — STANDARD ([A]/[F]/[R])

Use for phases that produce **reviewable artifacts** the developer can scan in 30 seconds and form an opinion on (Phase 2 DESIGN diagrams, Phase 3 SPECIFY prose, Phase 2 PLAN of implement-wizard). The current default behavior.

```
Phase {N}: {Name} complete.

Produced:
  - {artifact 1 with one-line summary}
  - {artifact 2 with one-line summary}

Review and choose:
  [A]ccept — Proceed to next phase
  [F]eedback — Provide feedback, I'll revise
  [R]eject — Start this phase over

Your choice:
```

### Gate type C — CRITICAL ([A]/[F]/[R] + review checklist)

Use for phases that produce **permanent or hard-to-fix artifacts**: Phase 4 PLAN of design-wizard (issue decomposition baked into immutable DESIGN.md), Phase 5 FINALIZE of design-wizard (immutability lock), Phase 8 COMMIT of implement-wizard (commit lands in history). The standard `[A]/[F]/[R]` choice but the prompt explicitly names the failure modes the developer should look for. Prevents reflex-yes by giving the eye specific things to check.

```
Phase {N}: {Name} complete.

Produced:
  - {artifact 1}
  - {artifact 2}

⚠️  Before accepting, verify:
  - {failure mode 1 specific to this phase}
  - {failure mode 2}
  - {failure mode 3}

These are the things that bite later if you accept silently. Phase-specific
checklists are listed under each phase below.

Review and choose:
  [A]ccept — Proceed (this artifact may be hard to undo)
  [F]eedback — Provide feedback, I'll revise
  [R]eject — Start this phase over

Your choice:
```

### Handle Feedback / Reject (all gate types)

If **Feedback**:
1. Developer provides specific feedback
2. AI revises based on feedback
3. Re-present with changes highlighted
4. Loop until Accept or Reject

If **Reject**:
1. Ask developer: "Start over from beginning of phase, or specific sub-step?"
2. Restart from specified point
3. Previous work is not lost (saved in temp state)

If **Accept** (or auto-proceed for LIGHT gates):
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
Run design wizard for {name}
    ↓
DESIGN.md + diagrams (class, package, sequence)
    ↓
TDD.md + EDGE-CASES.md
    ↓
Issue files (model, controller, view, tests, integration)
    ↓
Run implement wizard for {name}, issue=model
    ↓
Model implementation + tests
    ↓
Knowledge record (model.md updated)
    ↓
Run implement wizard for {name}, issue=controller
    ↓
Controller implementation + tests
    ↓
Knowledge record (controller.md updated)
    ↓
... (repeat for all issues)
    ↓
Run PR wizard for {name}, mode=pre-pr
    ↓
Validation report (PASS/FAIL)
    ↓
Developer creates PR
    ↓
Reviewers comment
    ↓
Run PR wizard for {name}, mode=feedback
    ↓
Feedback issues → fixes → knowledge records
    ↓
PR merged
    ↓
Run PR wizard for {name}, mode=capture
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
