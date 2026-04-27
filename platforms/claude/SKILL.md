---
name: magic-seed
description: Universal workflow generator. Detects your project type, asks about your needs, and generates tailored AI-assisted development wizards (design, implement, PR, test, deploy, docs) with living documentation and knowledge base support. Zero installation — everything is markdown that the AI reads and executes.
argument-hint: [command] [args]
---

# magic-seed (Claude Code wrapper)

This is the Claude Code packaging of magic-seed. The full platform-neutral workflow lives in `instructions.md` (one directory above this file once installed). **Read it before acting on any magic-seed intent.**

---

## Command alias table

When the developer uses one of these slash commands, treat it as the corresponding magic-seed intent and follow the workflow described in `instructions.md`.

| Slash command | Intent |
|---------------|--------|
| `/magic-seed init` | Initialize magic-seed for this project |
| `/magic-seed wizard <name>` | Run a specific wizard (`<name>` is one of: design, implement, pr, test, deploy, docs) |
| `/magic-seed status` | Show project workflow status |
| `/magic-seed kb <query>` | Search the knowledge base |
| `/design-wizard <feature>` | Run the design wizard against `<feature>` |
| `/implement-wizard <feature>` | Run the implement wizard against `<feature>` |
| `/pr-wizard <feature>` | Run the PR wizard against `<feature>` |
| `/test-wizard <feature>` | Run the test wizard against `<feature>` |
| `/deploy-wizard <feature>` | Run the deploy wizard against `<feature>` |
| `/docs-wizard <feature>` | Run the docs wizard against `<feature>` |

If the developer expresses one of these intents in natural language ("design a feature called auth", "implement the next issue") instead of a slash command, treat it the same way.

---

## What this wrapper does

This file exists only to:

1. Provide the YAML frontmatter Claude Code uses to discover and trigger the skill.
2. Translate the slash-command syntax above into the platform-neutral intents that `instructions.md` describes.

All workflow logic — detection, discovery, profile loading, wizard generation, knowledge-base structure, review gates — lives in `instructions.md`. Do not duplicate it here.

## Init must persist plumbing, not just generate output

When the developer runs `/magic-seed init`, the agent must follow `instructions.md` Step 7 in full. A successful Claude Code init produces **all four** of the following, not just the last two:

1. **This wrapper installed:** `.claude/skills/magic-seed/SKILL.md` (symlink or copy of `platforms/claude/SKILL.md`).
2. **Universal entry point reachable:** `.ai-workflow/instructions.md`, `.ai-workflow/universal/`, and `.ai-workflow/profiles/{detected}/`.
3. **Rendered wizards:** `.ai-workflow/wizards/*.md` and (if needed) `.ai-workflow/rules.md`.
4. **Knowledge base scaffold:** `docs/project/`, `docs/team/`, `docs/features/` per `universal/knowledge-base-spec.md`.

Run `instructions.md` Step 8 (Verify Install) before reporting success.
