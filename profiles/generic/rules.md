# Rules: Generic

> **Applies to:** Generic profile projects  
> **Purpose:** Minimal rules that work for any tech stack

---

## Architecture Rules

### Rule G1: Separation of Concerns

Business logic must be separate from infrastructure and presentation.

**Rationale:** Makes testing easier and allows swapping implementations.

**Check:** Domain code doesn't import framework-specific libraries.

**Example:**
```
✓ Good: domain/OrderService.py (no Django imports)
✗ Bad: OrderService.py imports django.db
```

### Rule G2: One Responsibility Per Module

Each file/module/class should have one clear responsibility.

**Rationale:** Easier to understand, test, and maintain.

**Check:** Can you describe what this module does in one sentence?

---

## Code Style Rules

### Rule G3: Consistent Naming

Use the project's naming convention consistently.

**Discovery:** Determine convention from existing code:
- `PascalCase` for classes/types
- `camelCase` for variables/functions
- `snake_case` for variables/functions
- `kebab-case` for files

**Apply:** Use the same convention for new code.

### Rule G4: Clear Function Names

Function names should describe what they do.

**Good:** `calculateTotalPrice()`, `validateEmailFormat()`
**Bad:** `calc()`, `check()`, `doStuff()`

---

## Documentation Rules

### Rule G5: Document Public APIs

All public functions, classes, and modules must have documentation.

**Format:** Language-appropriate (docstrings, JSDoc, XML docs, etc.)

**Content:**
- What it does
- Parameters and types
- Return value and type
- Exceptions/errors
- Example usage

### Rule G6: Keep Documentation Close

Documentation lives with the code or in `flow-storage/tasks/{task-name}/`.

**Not in:** Separate wiki, external docs, or outdated files.

---

## Testing Rules

### Rule G7: Test Business Logic

All business logic must have unit tests.

**Minimum:** Happy path + error cases

### Rule G8: Tests are Documentation

Tests should show how to use the code.

**Good:** Test names describe the scenario: `test_order_calculation_with_discount()`
**Bad:** Test names are vague: `test1()`, `test_order()`

---

## Git Rules

### Rule G9: Atomic Commits

Each commit should be one logical change.

**Good:** `feat: add email validation to user registration`
**Bad:** `update` (contains multiple unrelated changes)

### Rule G10: Descriptive Messages

Commit messages should explain WHY, not just WHAT.

**Format:**
```
type(scope): short description

Longer explanation if needed:
- Why this change was made
- What problem it solves
- Any breaking changes

Refs: #123
```

---

## Security Rules

### Rule G11: No Secrets in Code

Never commit API keys, passwords, or tokens.

**Check:** Search for `password`, `secret`, `token`, `key` in code.
**Use:** Environment variables or secret management.

### Rule G12: Validate Inputs

All external inputs must be validated.

**Applies to:** User input, API requests, file reads, database queries

---

## Performance Rules

### Rule G13: Avoid Premature Optimization

Write clear code first. Optimize only when profiling shows a bottleneck.

**Check:** Is this optimization based on measurement or assumption?

### Rule G14: Efficient Algorithms

Use appropriate data structures and algorithms.

**Check:** Time and space complexity are reasonable for the use case.

---

## Validation Checklist

Before considering a task complete:

- [ ] Code follows naming conventions
- [ ] Public APIs are documented
- [ ] Business logic has tests
- [ ] No secrets in code
- [ ] Inputs are validated
- [ ] Commits are atomic and descriptive
- [ ] Documentation is updated
