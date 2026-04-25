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

## Step 2: Activate the wrapper for your AI platform

Follow the install guide for the platform you use:

- Claude Code → [`platforms/claude/README.md`](../platforms/claude/README.md)
- Cursor → [`platforms/cursor/README.md`](../platforms/cursor/README.md)
- GitHub Copilot → [`platforms/github-copilot/README.md`](../platforms/github-copilot/README.md)
- OpenCode → [`platforms/opencode/README.md`](../platforms/opencode/README.md)
- Kimi Code CLI → [`platforms/kimi-code/README.md`](../platforms/kimi-code/README.md)

The wrapper teaches your AI how to recognize magic-seed intents on your platform.

---

## Step 3: Initialize

Ask your AI to initialize magic-seed for the project (use the platform's invocation style — slash command on platforms that support them, natural language anywhere). The AI will:

1. Detect your project type
2. Ask a few questions
3. Generate wizards
4. Install them to `.ai-workflow/wizards/`

---

## Step 4: Design Your First Feature

Ask your AI: "Design a feature called my-feature."

The AI will:
1. Load your project context
2. Ask about the feature
3. Generate diagrams (class, package)
4. Create DESIGN.md, TDD.md, EDGE-CASES.md
5. Create implementation issues

**Review and approve each phase.**

---

## Step 5: Implement

Ask your AI: "Implement the next issue for my-feature."

The AI will:
1. Read the first issue
2. Plan implementation
3. Write code
4. Run validation
5. Present for your review

**Approve each issue before proceeding to the next.**

---

## Step 6: PR

After all issues are complete, ask your AI: "Validate my-feature for PR."

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
Check that `.ai-workflow/wizards/` exists after the AI finishes initialization.

### "Design phase stuck"
Type `A` or `Accept` explicitly. The AI waits for your approval.

---

## Tips

- **Be specific** when describing features
- **Iterate** — it's okay to reject and refine
- **Update docs** as you go — they're living documents
- **Use diagrams** — they catch misunderstandings early
