# How to Customize

Tailor ai-flow-anything to your project's specific needs.

---

## Customization Points

You can customize without modifying ai-flow-anything core:

| File | Purpose |
|------|---------|
| `.ai-workflow/rules.md` | Project-specific rules |
| `.ai-workflow/flows/*.md` | Custom or overridden flows |
| `flow-storage/project/CONVENTIONS.md` | Team coding standards |
| `flow-storage/project/PATTERNS.md` | Reusable patterns |
| `flow-storage/team/workflows.md` | Team processes |

---

## Adding Custom Rules

Create `.ai-workflow/rules.md`:

```markdown
# Custom Rules: {Project Name}

## {Category}

### Rule C1: {Name}
{Description}

**Why:** {Rationale}

**Check:** {Verification}

**Example:**
```
{code}
```
```

Rules here are merged with profile rules. In conflicts, custom rules win.

---

## Overriding Flows

To modify a flow's behavior:

1. Copy from profile (replace `<flow-name>` with the flow you want to override, e.g. `design-flow`):
   ```bash
   cp .ai-workflow/profiles/{type}/skeletons/<flow-name>.md .ai-workflow/flows/<flow-name>.md
   ```

2. Edit the copy:
   - Add phases
   - Remove phases
   - Change validation
   - Add tech-specific steps

3. AI will use your version instead of profile's.

---

## Example: Adding a Sub-task

The 2-phase, 2-gate default cadence is deliberate. Most customizations should add a **sub-task** to an existing phase, not a new top-level phase. Adding a phase adds a user-facing gate.

To add "Security Review" to design-flow, insert a sub-task into Phase 1: SPECIFY:

```markdown
### Phase 1: SPECIFY *(STANDARD gate)*

Sub-tasks (auto-proceed):
**1.1 UNDERSTAND** — ...
**1.2 DESIGN** — ...
**1.3 SPECIFY** — Write task-design.md / task-technical-design.md / task-edge-cases.md
**1.4 SECURITY REVIEW** — (NEW)
  - Check data flow for sensitive data
  - Verify authentication requirements
  - Check authorization boundaries
  - Review input validation needs
  - Append findings to task-design.md security section
```

If security review genuinely needs its own user-facing gate (e.g. you want a separate sign-off from a security reviewer), promote it to a third top-level phase — but only if that gate is justified.

---

## Example: Custom Validation

Add project-specific validation to implement-flow's Phase 1 sub-task 1.4 AUTO-VALIDATE:

```markdown
**1.4 AUTO-VALIDATE** — Run profile checks + project test suite. Additional project-specific checks:
- [ ] No hardcoded API keys
- [ ] All API calls have error handling
- [ ] User input is sanitized
- [ ] Database queries use parameterized statements
```

---

## Team Conventions

Document team-specific conventions in `flow-storage/project/CONVENTIONS.md`:

```markdown
# Conventions: {Team Name}

## Naming
- Components: `PascalCase`
- Utilities: `camelCase`
- Constants: `UPPER_SNAKE_CASE`

## Git
- Branch naming: `feature/{ticket}-{description}`
- Commit format: `type(scope): message`
- Squash before merge

## Code Review
- Minimum 1 approval
- CI must pass
- No merge conflicts
```

---

## Pattern Catalog

Document reusable patterns in `flow-storage/project/PATTERNS.md`:

```markdown
# Patterns

## Pattern: Repository with Cache
**Used by:** UserService, ProductService
**Context:** Data access with caching
**Solution:**
```typescript
class CachedRepository<T> {
  private cache = new Map<string, T>();
  
  async findById(id: string): Promise<T> {
    if (this.cache.has(id)) {
      return this.cache.get(id)!;
    }
    
    const item = await this.db.findById(id);
    this.cache.set(id, item);
    return item;
  }
}
```
**References:** `src/repositories/CachedRepository.ts`
```

---

## Team Workflows

Document how your team works in `flow-storage/team/workflows.md`:

```markdown
# Workflows

## Task Development
1. Create ticket in {tool}
2. Run the design flow
3. Get design approved by tech lead
4. Run the implement flow
5. Run the test flow
6. Create PR (minimum 1 reviewer)
7. Run the PR flow
8. Merge after approval

## Hotfix Process
1. Create branch from `main`
2. Fix issue
3. Test manually
4. Create PR with `hotfix:` prefix
5. Fast-track review
6. Merge and deploy

## Release Schedule
- Weekly releases on Tuesdays
- Feature freeze Monday EOD
- Deploy Tuesday morning
- Monitor for 24 hours
```

---

## Environment-Specific Customization

Different environments may need different rules:

```markdown
# Environment Rules

## Development
- Debug logging enabled
- Mock external services
- Hot reload active

## Staging
- Production-like data
- External services (sandbox)
- Performance monitoring

## Production
- No debug logging
- Real external services
- Error alerting
```

---

## Tips

- **Document everything** — If it's not documented, it doesn't exist
- **Review regularly** — Update conventions as team evolves
- **Get consensus** — Team should agree on conventions
- **Start minimal** — Add rules as needed, not all at once
- **Use examples** — Show correct/incorrect code
- **Link to patterns** — Reference PATTERNS.md from code
- **Keep living** — Update when practices change

---

## Migration from Other Tools

If migrating from another workflow tool:

1. Export existing conventions
2. Map to ai-flow-anything structure:
   - Conventions → `flow-storage/project/CONVENTIONS.md`
   - Patterns → `flow-storage/project/PATTERNS.md`
   - Rules → `.ai-workflow/rules.md`
3. Adapt flows to match existing workflow
4. Train team on new commands
