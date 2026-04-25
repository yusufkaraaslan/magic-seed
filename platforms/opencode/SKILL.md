---
name: magic-seed
description: Universal workflow generator. Detects project type, asks the developer about goals, and generates tailored AI-assisted development wizards (design, implement, PR, test, deploy, docs) with living documentation and a knowledge base. Activate when the developer asks to initialize magic-seed, design or implement a feature, run a wizard, validate for PR, capture lessons learned, or search the project knowledge base.
license: MIT
metadata:
  audience: developers
  workflow: feature-development
---

# magic-seed (OpenCode wrapper)

This is the OpenCode packaging of magic-seed. The full platform-neutral workflow lives in `instructions.md` at the magic-seed installation root (typically `.ai-workflow/instructions.md` in the user's project). **Read it before acting on any magic-seed intent.**

---

## When to apply this skill

OpenCode loads this skill on demand based on the `description` above. Typical triggers:

- "Initialize magic-seed for this project"
- "Design a feature called <name>"
- "Implement the next issue for <feature>"
- "Validate <feature> for PR"
- "Process PR feedback for <feature>"
- "Capture lessons learned for <feature>"
- "Search the knowledge base for <query>"

If the developer expresses any of these intents, translate it to the corresponding magic-seed action and follow `instructions.md`.

---

## What this wrapper does

This file exists only to:

1. Provide the YAML frontmatter OpenCode uses to discover and load the skill on demand.
2. Point the agent at `instructions.md` for the full workflow logic.

All workflow logic — detection, discovery, profile loading, wizard generation, knowledge-base structure, review gates — lives in `instructions.md`. Do not duplicate it here.
