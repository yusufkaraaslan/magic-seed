# Quick Start

Get magic-seed running in 5 minutes.

---

## Step 1: Clone magic-seed

```bash
cd your-project
git clone https://github.com/yourusername/magic-seed.git .ai-workflow
```

Or download and extract to `.ai-workflow/`.

---

## Step 2: Initialize

Open your AI assistant (Claude, Cursor, etc.) and type:

```
/magic-seed init
```

The AI will:
1. Detect your project type
2. Ask a few questions
3. Generate wizards
4. Install them to `.ai-workflow/wizards/`

---

## Step 3: Design Your First Feature

```
/design-wizard my-feature
```

The AI will:
1. Load your project context
2. Ask about the feature
3. Generate diagrams (class, package)
4. Create DESIGN.md, TDD.md, EDGE-CASES.md
5. Create implementation issues

**Review and approve each phase.**

---

## Step 4: Implement

```
/implement-wizard my-feature
```

The AI will:
1. Read the first issue
2. Plan implementation
3. Write code
4. Run validation
5. Present for your review

**Approve each issue before proceeding to the next.**

---

## Step 5: PR

After all issues are complete:

```
/pr-wizard my-feature
```

The AI will:
1. Validate everything
2. Check tests
3. Verify documentation
4. Give you a go/no-go

---

## Next Steps

- [Read how to customize](how-to-customize.md)
- [Read how to add custom wizards](how-to-add-wizard.md)
- [Read architecture explained](architecture-explained.md)

---

## Troubleshooting

### "Profile not detected"
The AI will ask you to choose a profile or create a custom one. Pick the closest match.

### "Wizards not generated"
Check that `.ai-workflow/wizards/` exists after `/magic-seed init`.

### "Design phase stuck"
Type `A` or `Accept` explicitly. The AI waits for your approval.

---

## Tips

- **Be specific** when describing features
- **Iterate** — it's okay to reject and refine
- **Update docs** as you go — they're living documents
- **Use diagrams** — they catch misunderstandings early
