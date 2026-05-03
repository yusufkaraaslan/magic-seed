# How to Customize

Tailor magic-seed to your project's specific needs.

---

## Customization Points

You can customize without modifying magic-seed core:

| File | Purpose |
|------|---------|
| `.ai-workflow/rules.md` | Project-specific rules |
| `.ai-workflow/wizards/*.md` | Custom or overridden wizards |
| `docs/project/CONVENTIONS.md` | Team coding standards |
| `docs/project/PATTERNS.md` | Reusable patterns |
| `docs/team/workflows.md` | Team processes |

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

## Overriding Wizards

To modify a wizard's behavior:

1. Copy from profile:
   ```bash
   cp .ai-workflow/profiles/{type}/skeletons/design-wizard.md .ai-workflow/wizards/design-wizard.md
   ```

2. Edit the copy:
   - Add phases
   - Remove phases
   - Change validation
   - Add tech-specific steps

3. AI will use your version instead of profile's.

---

## Example: Adding a Phase

Add "Security Review" to design-wizard:

```markdown
### Phase 4.5: SECURITY REVIEW
**Purpose:** Review design for security implications.

**Steps:**
1. Check data flow for sensitive data
2. Verify authentication requirements
3. Check authorization boundaries
4. Review input validation needs

**Artifacts:**
- Security notes in DESIGN.md

**Gate:** [A]ccept / [F]eedback / [R]eject
```

---

## Example: Custom Validation

Add project-specific validation to implement-wizard:

```markdown
### Phase 4: AUTO-VALIDATE

Additional checks:
- [ ] No hardcoded API keys
- [ ] All API calls have error handling
- [ ] User input is sanitized
- [ ] Database queries use parameterized statements
```

---

## Team Conventions

Document team-specific conventions in `docs/project/CONVENTIONS.md`:

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

Document reusable patterns in `docs/project/PATTERNS.md`:

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

Document how your team works in `docs/team/workflows.md`:

```markdown
# Workflows

## Feature Development
1. Create ticket in {tool}
2. Run `/design-wizard`
3. Get design approved by tech lead
4. Run `/implement-wizard`
5. Run `/test-wizard`
6. Create PR (minimum 1 reviewer)
7. Run `/pr-wizard`
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
2. Map to magic-seed structure:
   - Conventions → `docs/project/CONVENTIONS.md`
   - Patterns → `docs/project/PATTERNS.md`
   - Rules → `.ai-workflow/rules.md`
3. Adapt wizards to match existing workflow
4. Train team on new commands
