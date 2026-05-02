# How to Add a Flow

Extend ai-flow-anything with custom flow types.

---

## When to Add a Flow

Add a flow when:
- Your workflow has steps not covered by existing flows
- Your team has unique processes
- You want to automate a recurring task

Examples:
- Security audit flow
- Performance optimization flow
- Database migration flow
- Localization flow
- Accessibility audit flow

---

## Flow Structure

```markdown
# Flow: {Name}

## Purpose
What this flow does.

## When to Use
Conditions for using this flow.

## Input
What the flow needs.

## Output
What the flow produces.

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

## Example: Security Audit Flow

```markdown
# Flow: Security Audit

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
- Updated task flow files

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

1. Create file `.ai-workflow/flows/{name}-flow.md`
2. Write flow following structure above
3. Use in AI assistant:
   ```
   /{name}-flow
   ```

---

## Adding to Profile

To share with profile users:

1. Create file `profiles/{type}/skeletons/{name}-flow.md`
2. Add the flow to the profile's README capability table
3. Document the new flow's intent in `instructions.md` if it should be discoverable globally; otherwise the profile's skeletons surface it during initialization

---

## Tips

- **Start simple** — 2-3 phases are enough
- **Reuse patterns** — Copy structure from existing flows
- **Be specific** — Tell AI exactly what to check
- **Include gates** — Every phase needs review
- **Document output** — What files are created
- **Test it** — Run on real code before relying on it

---

## Common Custom Flows

| Flow | Purpose | When |
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

## Integration with Existing Flows

Custom flows can integrate with the main workflow:

```markdown
## Integration

Run this flow after the implement flow and before the PR flow:

```
design flow  →  implement flow  →  security-audit flow  →  PR flow
```

If security audit fails, fix issues before PR.
```
