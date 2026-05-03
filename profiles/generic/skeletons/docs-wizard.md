# Wizard: Docs

> **Profile:** Generic  
> **Purpose:** Maintain and update project documentation

---

## When to Use

- Documentation is outdated
- New team member joining
- After major architecture changes
- Periodic documentation review

## Input

- Existing docs in `docs/`
- Source code
- Knowledge base records

## Output

- Updated documentation
- New documentation
- Removed outdated docs

---

## Phases

### Phase 1: AUDIT

**Purpose:** Assess current documentation state.

**Steps:**
1. List all documentation:
   - `docs/project/*.md`
   - `docs/features/*/*.md`
   - `docs/team/*.md`
   - `README.md`
   - `CHANGELOG.md`
   - Any other `.md` files
2. Check for:
   - Outdated content (references old versions)
   - Missing documentation (new features without docs)
   - Broken links
   - Placeholder text (`{placeholder}`, `TODO`, `FIXME`)
3. Compare docs to codebase:
   - Features documented vs features implemented
   - APIs documented vs APIs existing

**Artifacts:**
- Audit report:
  ```
  Documentation Audit
  
  Total docs: 25
  Up to date: 18
  Outdated: 4
  Missing: 3
  Broken links: 2
  
  Outdated:
    - docs/project/ARCHITECTURE.md (references v1.0)
    - docs/features/old-feature/DESIGN.md (feature removed)
  
  Missing:
    - docs/features/new-feature/ (no docs)
    - API docs for /users endpoint
  ```

**Gate:** [A]ccept audit / [M]odify scope / [R]eject

---

### Phase 2: UPDATE

**Purpose:** Refresh outdated documentation.

**Steps:**
1. For each outdated doc:
   - Read current version
   - Read relevant code
   - Update content
   - Update diagrams
   - Update links
2. Fix broken links
3. Remove placeholder text
4. Update code examples to match current code

**Artifacts:**
- Updated documentation files

**Gate:** [A]ccept / [F]eedback / [R]eject

---

### Phase 3: CREATE

**Purpose:** Add missing documentation.

**Steps:**
1. For each missing doc:
   - Determine scope
   - Read related code
   - Write documentation
   - Add diagrams if needed
2. Create feature docs for undocumented features:
   ```
   docs/features/{feature-name}/
   ├── DESIGN.md
   ├── TDD.md
   └── README.md
   ```
3. Add API documentation
4. Add architecture decision records (ADRs)

**Artifacts:**
- New documentation files

**Gate:** [A]ccept / [F]eedback / [R]eject

---

### Phase 4: ORGANIZE

**Purpose:** Improve documentation structure.

**Steps:**
1. Check navigation:
   - Can new developers find what they need?
   - Are related docs linked?
2. Add cross-references:
   - Link related docs
   - Add "See also" sections
3. Improve discoverability:
   - Add table of contents
   - Add search keywords
   - Standardize headings
4. Archive or remove:
   - Docs for removed features
   - Outdated ADRs

**Artifacts:**
- Reorganized docs
- Updated README with navigation

**Gate:** [A]ccept / [F]eedback / [R]eject

---

### Phase 5: VERIFY

**Purpose:** Ensure documentation quality.

**Steps:**
1. Check completeness:
   - [ ] All features documented
   - [ ] All APIs documented
   - [ ] All ADRs present
2. Check accuracy:
   - [ ] Code examples work
   - [ ] Diagrams match code
   - [ ] Links work
3. Check readability:
   - [ ] Clear headings
   - [ ] Good examples
   - [ ] No jargon without explanation
4. Check consistency:
   - [ ] Same terms used throughout
   - [ ] Same formatting
   - [ ] Same structure

**Artifacts:**
- Verification report

**Gate:** [A]ccept / [F]ix issues / [R]eject

---

## Documentation Standards

### File Organization

```
docs/
├── README.md              # Documentation index
├── project/
│   ├── ARCHITECTURE.md
│   ├── CONVENTIONS.md
│   ├── PATTERNS.md
│   └── DECISIONS.md
├── features/
│   └── {feature-name}/
│       ├── DESIGN.md
│       ├── TDD.md
│       ├── EDGE-CASES.md
│       └── README.md
├── team/
│   ├── onboarding.md
│   ├── workflows.md
│   └── glossary.md
└── api/
    └── README.md
```

### Naming

- Use `lowercase-with-dashes.md`
- `README.md` for directory overviews
- Be descriptive: `authentication-flow.md` not `auth.md`

### Structure

```markdown
# Title

> **Status:** Draft | Current | Outdated  
> **Last Updated:** YYYY-MM-DD

## Overview
Brief description.

## Details
...

## Examples
```code
...
```

## See Also
- [Related Doc](../related.md)
```

---

## Customization

Users can customize:
- Add documentation categories
- Change verification checklist
- Add style guide enforcement
- Add diagram generation
- Add auto-linking
