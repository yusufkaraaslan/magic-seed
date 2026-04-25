# Universal Rules

> **Applies to:** ALL projects, ALL profiles, ALL wizards  
> **Purpose:** Non-negotiable guardrails that never change regardless of tech stack

---

## Rule 1: Documentation Before Code

No implementation may begin without an approved design document.

**Enforcement:**
- The design wizard MUST run before the implement wizard
- The implement wizard checks for `DESIGN.md` before starting
- If `DESIGN.md` is missing, the implement wizard exits with: "Run the design wizard for `{name}` first"

**Exception:** Hotfixes and one-line changes may skip design phase, but must be documented retroactively.

---

## Rule 2: Living Documents

All documentation in `docs/` is living — updated at every workflow step, never archived.

**Practices:**
- DESIGN.md is locked after sign-off (immutable), but deviations are recorded in TDD.md
- TDD.md evolves during implementation
- EDGE-CASES.md grows as new edge cases are discovered
- Issues transform into knowledge records after completion
- Feedback issues become permanent knowledge records

**Anti-pattern:** Creating `docs-v2/`, `docs-archive/`, or dated copies. Update in place.

---

## Rule 3: Unlimited Iteration Review Gates

Every phase ends with developer review. Options are always:

- **[A]ccept** — Proceed to next phase
- **[F]eedback** — Provide feedback, wizard revises and re-presents
- **[R]eject** — Abandon this approach, start over

**Constraints:**
- No maximum iteration count
- Never auto-accept even if "perfect"
- Developer must explicitly type "A" or "Accept"
- Feedback must be specific and actionable

---

## Rule 4: Diagrams Are Mandatory

Every feature design must produce at minimum:

1. **Class diagram** — Core abstractions and relationships
2. **Package/module diagram** — Dependencies and layering
3. **Sequence diagram** — For any non-trivial flow (optional for trivial features)

**Standards:**
- Follow `universal/diagram-standards.md`
- Default format: PlantUML
- User can override per project
- Diagrams are reviewed alongside text documents
- Diagrams live in `docs/features/{name}/diagrams/`

---

## Rule 5: Knowledge Capture

After every feature, the following must be updated:

**Per-feature:**
- `lessons-learned.md` — What went well, what was missed
- `feedback/` — PR feedback records (if any)

**Project-level:**
- `docs/project/PATTERNS.md` — New reusable patterns discovered
- `docs/project/DECISIONS.md` — Architecture decisions (ADRs)

**Team-level:**
- `docs/team/workflows.md` — Process improvements

---

## Rule 6: No Auto-Approval

The AI never decides a phase is "good enough" without developer input.

**Anti-patterns:**
- "This looks good, proceeding..."
- "Implementation complete, moving to PR..."
- Auto-accepting after N iterations

**Correct:**
- "Phase complete. Review and type [A]ccept, [F]eedback, or [R]eject"
- Waiting for explicit developer input before proceeding

---

## Rule 7: Atomic Commits

Each issue (from implement-wizard) becomes one commit.

**Format:**
```
feat({feature-name}): {issue-type} — brief description

- Detailed change 1
- Detailed change 2

Refs: #{issue-number}
```

**Example:**
```
feat(user-authentication): implement login validation

- Add UserValidator with email format checking
- Implement password strength requirements
- Add session token generation

Refs: #124
```

---

## Rule 8: Issue Frontmatter Schema

All generated issue files must use this frontmatter:

```yaml
---
issue: {issue-name}
feature: "{feature-name}"
status: pending          # pending | in_progress | complete
depends-on: [{dependencies}]
accepted-date: null       # set when developer accepts implementation
---
```

**Fields:**
- `issue` — Short identifier (kebab-case, e.g., user-session, auth-service, login-form)
- `feature` — Full feature name (kebab-case or PascalCase)
- `status` — Current state
- `depends-on` — List of issue names this depends on
- `accepted-date` — Date of developer acceptance (ISO 8601)

---

## Rule 9: Design Lock After Sign-Off

After developer accepts DESIGN.md, it becomes immutable:

```markdown
> **Status:** v2.0 (ENHANCED) — Signed Off
> **Signed Off By:** {developer name}
> **Date:** {date}
> **Immutable:** Yes
```

**Implications:**
- Changes after lock require new version or new feature
- Deviations during implementation are recorded in TDD.md, not DESIGN.md
- Post-merge, edge cases and deviations feed into next iteration

---

## Rule 10: Context Loading First

Every wizard must load project context before doing anything else.

**Context sources (in order):**
1. `docs/project/ARCHITECTURE.md` — Overall architecture
2. `docs/project/CONVENTIONS.md` — Coding standards
3. `docs/project/PATTERNS.md` — Reusable patterns
4. Prior feature docs in `docs/features/` — Reference implementations
5. `.ai-workflow/rules.md` — Project-specific overrides

**Loading pattern:**
```markdown
## First Step: Load Context

Before doing anything else, read:
1. `docs/project/ARCHITECTURE.md`
2. `docs/project/CONVENTIONS.md`
3. `docs/project/PATTERNS.md`

This provides: architecture rules, framework patterns, coding standards, 
and reference implementations. **Do not skip this step.**
```

---

## Rule 11: Transparent Execution

The AI must show what it's doing, not just do it.

**Required transparency:**
- List files being read
- Show key decisions made
- Display progress through phases
- Explain why a pattern was chosen
- Cite reference implementations

**Example:**
```
Phase 1: READ complete

Issue: controller.md
  Requirements: 4 acceptance criteria extracted
  Files to create: 5 identified

Design docs loaded:
  DESIGN.md Section 4.3 (Controller Layer)
  TDD.md Controller section
  EDGE-CASES.md: 3 relevant edge cases

Reference implementation:
  OrderController.{{ext}} (API controller pattern)
  UserService.{{ext}} (service layer pattern)

Proceeding to Phase 2: PLAN...
```

---

## Rule 12: No Placeholders in Final Output

After generation, verify no `{placeholder}` text remains in non-template files.

**Check:** Search for `\{[a-z][a-z0-9_-]*\}` pattern in generated files.
**Exception:** Template files meant for developer customization (e.g., `DESIGN-TEMPLATE.md`)

---

## Rule 13: Test Coverage for New Features

Every feature must have tests.

**Minimum:**
- Unit tests for business logic
- Integration tests for system boundaries
- Edge case tests from EDGE-CASES.md

**Coverage target:** 80% for new code (profile may override)

---

## Rule 14: Backward Compatibility

Changes must not break existing features.

**Checks:**
- Run existing tests before and after changes
- Verify no regressions in related features
- Update integration points, don't break them

---

## Rule 15: Accessibility and Inclusivity

(For UI projects) All user-facing features must consider accessibility.

**Checks:**
- Color contrast
- Keyboard navigation
- Screen reader compatibility
- Touch target sizes

---

## Enforcement

These rules are enforced by:
1. **Wizards** — Check rules at each phase gate
2. **Validators** — Run automated checks where possible
3. **Developer review** — Human verifies at [A]/[F]/[R] gates
4. **Knowledge base** — Violations recorded in lessons-learned

**Severity levels:**
- **error** — Must fix before proceeding (Rule 1, 2, 4, 6, 8, 9)
- **warning** — Should fix, can proceed with justification (Rule 3, 7, 10, 11)
- **info** — Best practice, noted but not blocking (Rule 5, 12, 13, 14, 15)
