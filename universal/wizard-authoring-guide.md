# Wizard Authoring Guide

> **Applies to:** Profile authors, advanced users  
> **Purpose:** How to write wizard skeletons that AI assistants can execute

---

## What is a Wizard?

A wizard is a markdown file that an AI assistant reads and follows to complete a complex, multi-step task. Think of it as a recipe that the AI cooks from.

**Key characteristics:**
- Written in natural language (prose, not code)
- Divided into sequential phases
- Each phase ends with a review gate
- Specifies input, output, and artifacts
- Contains instructions the AI interprets and executes

---

## Wizard Structure

Every wizard file follows this structure:

```markdown
# Wizard: {Name}

## Purpose
One-paragraph description of what this wizard does.

## When to Use
Conditions under which this wizard should be invoked.

## Input
What the wizard needs to start (files, context, prior work).

## Output
What the wizard produces (files, code, docs).

## Prerequisites
What must be true before this wizard can run.

## Phases

### Phase 1: {Name}
**Purpose:** What this phase accomplishes.

**Steps:**
1. Step description (natural language instruction)
2. Step description
3. Step description

**Artifacts:**
- File path and description

**Gate:** [A]ccept / [F]eedback / [R]eject

### Phase 2: {Name}
...

## Customization
How users can customize this wizard for their project.

## Example
Walkthrough of a typical execution.
```

---

## Writing Phases

### Phase Purpose

Start each phase with a clear purpose statement:

```markdown
### Phase 3: IMPLEMENT

**Purpose:** Write the source code for this issue, following project 
conventions and reference implementations.
```

### Phase Steps

Write steps as natural language instructions. The AI interprets them.

**Good:**
```markdown
**Steps:**
1. Read the issue file to extract acceptance criteria
2. Find 1-2 reference implementations of similar features
3. Read the reference implementations to understand patterns
4. Create the new files following the same patterns
5. Ensure all acceptance criteria are met
```

**Bad:**
```markdown
**Steps:**
1. grep "AC" issue.md
2. ls src/features/
3. cat reference1.{{ext}}
4. cp template.{{ext}} new.{{ext}}
```

The AI decides HOW to execute. You describe WHAT needs to happen.

### Phase Artifacts

List all files created or modified in this phase:

```markdown
**Artifacts:**
- `{{source-dir}}/features/{name}/Model/{Name}Model.{{ext}}` — Domain model
- `{{source-dir}}/features/{name}/Controller/{Name}Controller.{{ext}}` — Business logic
- `docs/features/{name}/issues/{issue-name}.md` — Updated knowledge record
```

### Phase Gate

Every phase must end with a review gate:

```markdown
**Gate:**

Present to developer:
- Summary of what was done
- Key decisions made
- Files created/modified
- Any warnings or issues

Ask:
"Review and choose: [A]ccept / [F]eedback / [R]eject"
```

---

## Writing Instructions

### Be Specific About Intent

Tell the AI what you're trying to achieve, not how to do it.

**Good:**
> "Ensure the model follows the project's naming conventions and uses the base class discovered in the slot catalog."

**Bad:**
> "Name the class PascalCase and extend BaseModel."

### Reference Context

Always tell the AI to load context first:

```markdown
## First Step: Load Context

Before doing anything else, read:
1. `docs/project/ARCHITECTURE.md`
2. `docs/project/CONVENTIONS.md`
3. `docs/project/PATTERNS.md`

This provides architecture rules, framework patterns, coding standards, 
and reference implementations.
```

### Cite Examples

When describing patterns, tell the AI to find examples:

```markdown
**Step 2: Find Reference Implementations**

Search the codebase for similar features. Look for:
- Features with similar complexity
- Features in the same domain
- Recently completed features (more likely to follow current conventions)

Read 1-2 reference implementations to understand:
- Class structure
- Lifecycle patterns
- Event handling
- Testing approach
```

### Handle Ambiguity

When something might vary, tell the AI to ask:

```markdown
**Step 3: Determine Async Pattern**

Check the codebase for async patterns:
- Search for `async/await` usage
- Search for Promise patterns
- Search for callback patterns

If multiple patterns are found, ask the developer:
"This project uses both async/await and Promises. Which pattern 
should I use for this feature?"
```

---

## Slot Usage

Slots are placeholders for project-specific values discovered during magic-seed initialization.

### Referencing Slots

In skeletons, use `{{slot-name}}` notation:

```markdown
## Step 1: Create Model

Create `{{source-dir}}/Model/{{Feature}}Model.{{ext}}` extending 
`{{base-model-class}}`.

Follow the pattern from `{{reference-implementation}}`.
```

### Slot Resolution

During generation, the AI replaces `{{slot-name}}` with discovered values:
- `{{source-dir}}` → `src/` or `Assets/Scripts/` or `lib/`
- `{{base-model-class}}` → `BaseModel` or `AbstractEntity`
- `{{reference-implementation}}` → `UserModel.{{ext}}` (varies by language)

### Fallback

If a slot is empty, the AI asks the developer:

```markdown
**Fallback:** If `{{base-model-class}}` is not discovered, ask:
"What is the base class for models in this project?"
```

---

## Rule Integration

Wizards must reference rules from two sources:

### Universal Rules

Always apply rules from `universal/rules.md`:

```markdown
**Apply Universal Rules:**
- Documentation before code (Rule 1)
- Unlimited iterations (Rule 3)
- No auto-approval (Rule 6)
- Transparent execution (Rule 11)
```

### Profile Rules

Apply profile-specific rules:

```markdown
**Apply Profile Rules:**
- No framework imports in domain layer (from `profiles/{type}/rules.md`)
- Use project's async pattern
- Follow naming conventions from `CONVENTIONS.md`
```

### Custom Rules

Check for project-specific overrides:

```markdown
**Check Custom Rules:**
Read `.ai-workflow/rules.md` if it exists. Apply any project-specific 
rules defined there.
```

---

## Common Patterns

### Resume Logic

Support resuming across sessions:

```markdown
## Resume Logic

Before starting, check for existing progress:
1. Read all issue files in `docs/features/{name}/issues/`
2. Find first issue with `status: pending` or `status: in_progress`
3. If found, ask developer: "Resume from {issue-name}?"
4. If developer confirms, skip completed issues

**Status display:**
```
Implementation Status: {feature-name}

  model.md       complete   (2026-04-23)
  controller.md  in_progress
  view.md        pending    (blocked by: controller)

Progress: 1/3 complete
```
```

### Error Handling

Handle failures gracefully:

```markdown
## Error Handling

If a step fails:
1. Report the failure to developer with context
2. Suggest alternatives
3. Ask: "Continue with alternative, retry, or abort?"

**Never silently skip errors.**
```

### Progress Tracking

Show progress through phases:

```markdown
## Progress Display

After each phase, show:
```
Phase {N}: {Name} — COMPLETE ✓

Artifacts:
  - {file-path-1}
  - {file-path-2}

Next: Phase {N+1}: {Next Name}
```
```

---

## Testing Wizards

Before publishing a profile, test the wizard:

### Self-Review Checklist

- [ ] Each phase has a clear purpose
- [ ] Steps are natural language, not pseudo-code
- [ ] Each phase ends with a gate
- [ ] Artifacts are listed
- [ ] Resume logic is described
- [ ] Error handling is described
- [ ] Context loading is first step
- [ ] Rules are referenced
- [ ] Slots are used for project-specific values
- [ ] Example walkthrough is provided

### Test Execution

1. Create a test project
2. Initialize magic-seed for it
3. Run each wizard on a sample feature
4. Verify output matches expectations
5. Check that gates work correctly
6. Verify knowledge base is populated

---

## Example: Minimal Wizard

```markdown
# Wizard: Code Review

## Purpose
Review code changes for quality, conventions, and potential issues.

## When to Use
Before committing code or creating a PR.

## Input
- Changed files (staged or unstaged)
- Project conventions from `docs/project/CONVENTIONS.md`

## Output
- Review report with findings
- Suggested fixes

## Prerequisites
- Git repository
- Changes to review

## Phases

### Phase 1: READ
**Purpose:** Understand what changed.

**Steps:**
1. Show git diff or changed files
2. Read relevant conventions
3. Identify which files need review

**Artifacts:**
- Review scope summary

**Gate:** Confirm scope — [A]ccept / [M]odify scope / [R]eject

### Phase 2: REVIEW
**Purpose:** Check code against conventions.

**Steps:**
1. Check naming conventions
2. Check code style
3. Check for common issues
4. Check test coverage
5. Verify documentation updates

**Artifacts:**
- Review report with findings

**Gate:** Present findings — [A]ccept / [F]eedback / [R]eject

### Phase 3: FIX
**Purpose:** Apply suggested fixes.

**Steps:**
1. Present fix suggestions with before/after
2. Developer chooses which to apply
3. Apply approved fixes
4. Re-run checks

**Artifacts:**
- Fixed code
- Updated review report

**Gate:** Confirm fixes — [A]ccept / [F]eedback / [R]eject
```

---

## Advanced: Conditional Logic

Wizards can have conditional behavior based on project state:

```markdown
## Conditional: Feature Type

If feature involves data models:
  - Include ERD diagram
  - Check database migrations

If feature involves UI:
  - Include accessibility checks
  - Check responsive design

If feature involves API:
  - Include endpoint documentation
  - Check auth requirements
```

The AI evaluates conditions and adapts the workflow.

---

## Best Practices

1. **Start simple.** A wizard with 3 clear phases is better than 8 confusing ones.
2. **Be explicit about gates.** Never let the AI proceed without developer input.
3. **Reference existing docs.** Don't duplicate information from CONVENTIONS.md or PATTERNS.md.
4. **Use examples.** Show the AI what good output looks like.
5. **Handle edge cases.** What if there's no reference implementation? What if the developer rejects?
6. **Keep it human-readable.** Other developers will read these files.
7. **Test incrementally.** Write one phase, test it, then add the next.
