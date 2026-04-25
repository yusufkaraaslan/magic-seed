# How to Add a Wizard

Extend magic-seed with custom wizard types.

---

## When to Add a Wizard

Add a wizard when:
- Your workflow has steps not covered by existing wizards
- Your team has unique processes
- You want to automate a recurring task

Examples:
- Security audit wizard
- Performance optimization wizard
- Database migration wizard
- Localization wizard
- Accessibility audit wizard

---

## Wizard Structure

```markdown
# Wizard: {Name}

## Purpose
What this wizard does.

## When to Use
Conditions for using this wizard.

## Input
What the wizard needs.

## Output
What the wizard produces.

## Prerequisites
What must be true before running.

## Phases

### Phase 1: {Name}
**Purpose:** What this phase does.

**Steps:**
1. Step description
2. Step description

**Artifacts:**
- File path and description

**Gate:** [A]ccept / [F]eedback / [R]eject

### Phase 2: {Name}
...

## Customization
How users can customize.
```

---

## Example: Security Audit Wizard

```markdown
# Wizard: Security Audit

## Purpose
Audit code for security vulnerabilities.

## When to Use
Before major releases or after security incidents.

## Input
- Source code
- Dependency list
- API endpoints

## Output
- Security report
- Fix recommendations
- Updated issue files

## Prerequisites
- Code is committed
- Dependencies are listed

## Phases

### Phase 1: SCAN
**Purpose:** Find potential vulnerabilities.

**Steps:**
1. Search for hardcoded secrets (API keys, passwords)
2. Check for SQL injection risks
3. Verify input validation
4. Check authentication coverage
5. Review authorization logic

**Artifacts:**
- `docs/security/audit-{date}.md`

**Gate:** Confirm scan complete — [A]ccept / [R]e-scan

### Phase 2: ASSESS
**Purpose:** Categorize findings by severity.

**Steps:**
1. Categorize each finding:
   - Critical: Data exposure, RCE
   - High: Auth bypass, injection
   - Medium: XSS, CSRF
   - Low: Info disclosure
2. Map to OWASP Top 10
3. Check compliance requirements (GDPR, SOC2)

**Artifacts:**
- Risk assessment matrix

**Gate:** [A]ccept / [F]eedback / [R]eject

### Phase 3: REMEDIATE
**Purpose:** Fix or mitigate findings.

**Steps:**
1. Generate fix for each finding
2. Present fix with before/after
3. Developer approves each fix
4. Apply approved fixes
5. Re-scan to verify

**Artifacts:**
- Fixed code
- `docs/security/remediation-{date}.md`

**Gate:** [A]ccept / [F]eedback / [R]eject

### Phase 4: DOCUMENT
**Purpose:** Record security posture.

**Steps:**
1. Update security documentation
2. Record decisions (won't fix, accepted risk)
3. Update threat model
4. Schedule next audit

**Artifacts:**
- Updated security docs
- `docs/security/audit-log.md`

**Gate:** [A]ccept / [R]eject
```

---

## Adding to Your Project

1. Create file `.ai-workflow/wizards/{name}-wizard.md`
2. Write wizard following structure above
3. Use in AI assistant:
   ```
   /{name}-wizard
   ```

---

## Adding to Profile

To share with profile users:

1. Create file `profiles/{type}/skeletons/{name}-wizard.md`
2. Add the wizard to the profile's README capability table
3. Document the new wizard's intent in `instructions.md` if it should be discoverable globally; otherwise the profile's skeletons surface it during initialization

---

## Tips

- **Start simple** — 2-3 phases are enough
- **Reuse patterns** — Copy structure from existing wizards
- **Be specific** — Tell AI exactly what to check
- **Include gates** — Every phase needs review
- **Document output** — What files are created
- **Test it** — Run on real code before relying on it

---

## Common Custom Wizards

| Wizard | Purpose | When |
|--------|---------|------|
| **security-audit** | Find vulnerabilities | Before releases |
| **perf-optimize** | Performance improvements | When slow |
| **db-migrate** | Database migrations | Schema changes |
| **i18n** | Internationalization | Adding languages |
| **a11y-audit** | Accessibility check | Compliance |
| **refactor** | Code refactoring | Tech debt |
| **onboard** | New developer setup | Team growth |
| **release** | Release checklist | Deployments |

---

## Integration with Existing Wizards

Custom wizards can integrate with the main workflow:

```markdown
## Integration

Run this wizard after the implement wizard and before the PR wizard:

```
design wizard  →  implement wizard  →  security-audit wizard  →  PR wizard
```

If security audit fails, fix issues before PR.
```
