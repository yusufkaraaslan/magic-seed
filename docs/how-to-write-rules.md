# How to Write Rules

Define project conventions that flows enforce.

---

## Rule Structure

```markdown
# Rules: {Project/Profile}

## {Category} Rules

### Rule {ID}: {Name}
{Description}

**Why:** {Rationale}

**Check:** {How to verify}

**Example:**
```
{code example}
```
```

---

## Universal vs Profile vs Custom

| Level | Location | Scope |
|-------|----------|-------|
| Universal | `universal/rules.md` | All projects |
| Profile | `profiles/{type}/rules.md` | Projects of this type |
| Custom | `.ai-workflow/rules.md` | Your project only |

Custom rules override profile rules, which override universal rules.

---

## Writing Good Rules

### 1. Be Specific

**Bad:**
```markdown
### Rule: Good code
Write good code.
```

**Good:**
```markdown
### Rule C1: Function Length
Functions should be 20 lines or fewer.

**Why:** Shorter functions are easier to understand and test.

**Check:** Count lines in function body (excluding comments).

**Exception:** Switch statements and initialization functions may be longer.
```

### 2. Explain Why

Always include rationale:

```markdown
**Why:** Using `async/await` instead of callbacks improves readability 
and error handling. It also enables better stack traces.
```

### 3. Provide Examples

Show correct and incorrect:

```markdown
**Correct:**
```typescript
async function fetchData(): Promise<Data> {
  const response = await api.get('/data');
  return response.data;
}
```

**Incorrect:**
```typescript
function fetchData(callback: (data: Data) => void) {
  api.get('/data', (response) => {
    callback(response.data);
  });
}
```
```

### 4. Include Exceptions

```markdown
**Exception:** Legacy code may use callbacks. Don't refactor unless 
touching the code for other reasons.
```

---

## Rule Categories

### Architecture Rules

Define how code is structured:
- Layer separation
- Dependency direction
- Module boundaries

### Code Style Rules

Define how code looks:
- Naming conventions
- Formatting
- Comments

### Performance Rules

Define efficiency requirements:
- Algorithmic complexity
- Resource usage
- Caching strategies

### Security Rules

Define security requirements:
- Input validation
- Authentication
- Data protection

### Testing Rules

Define testing requirements:
- Coverage thresholds
- Test types
- Mocking strategies

---

## Custom Rules Example

Create `.ai-workflow/rules.md` in your project:

```markdown
# Custom Rules: My Project

## API Rules

### Rule C1: API Versioning
All endpoints must include version prefix.

**Why:** Enables backward compatibility.

**Check:** Path starts with `/v{N}/`

**Example:**
```
✓ GET /v1/users
✗ GET /users
```

## Frontend Rules

### Rule C2: Loading States
Every async operation must show loading state.

**Why:** Prevents user confusion.

**Check:** Component has `isLoading` state that shows spinner/skeleton.

## Database Rules

### Rule C3: Migrations
Every schema change requires migration.

**Why:** Keeps environments in sync.

**Check:** Migration file exists for schema change.
```

---

## Rule Enforcement

Rules are enforced by:
1. **Flows** — Check at phase gates
2. **Validators** — Automated checks
3. **Developer review** — Human verification
4. **Knowledge base** — Record violations

---

## Tips

- **Start with 5-10 rules** — Too many are overwhelming
- **Focus on high-impact** — Rules that prevent bugs
- **Make them measurable** — "Functions should be short" → "Functions < 20 lines"
- **Review regularly** — Remove outdated rules
- **Get team buy-in** — Rules work better when team agrees
