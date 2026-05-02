---
name: magic-seed
description: Orchestrator skill for the magic-seed feature-development workflow. Use this skill ONLY for (a) initializing magic-seed in a new project, (b) showing project workflow status across in-flight features, (c) searching the project knowledge base, or (d) when the developer's request is ambiguous about which phase wizard applies. For specific phase work — design, implement, test, PR validation, deploy, docs — prefer the dedicated per-wizard skills (design-wizard, implement-wizard, pr-wizard, test-wizard, deploy-wizard, docs-wizard). Magic-seed enforces design-before-code: no feature code without a signed-off DESIGN.md.
license: MIT
metadata:
  audience: developers
  workflow: feature-development
  role: orchestrator
---

# magic-seed (OpenCode wrapper)

This is the OpenCode packaging of magic-seed. The full platform-neutral workflow lives in `instructions.md` at the magic-seed installation root (typically `.ai-workflow/instructions.md` in the user's project). **Read it before acting on any magic-seed intent.**

If `instructions.md` is not reachable in the user's project (no `.ai-workflow/instructions.md`, no symlink), the install is incomplete — the previous init only generated rendered output and skipped the install plumbing. In that case, before doing anything else, perform Step 7 of `instructions.md` against the magic-seed source repo this wrapper is shipped from, then continue. Do not fall back to "improvise from the wizard files alone" — the universal rules are not in those files.

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

## Init must persist plumbing, not just generate output

When the developer says "initialize magic-seed for this project", the agent must follow `instructions.md` Step 7 in full. Specifically: a successful OpenCode init produces **all four** of the following, not just the last two:

1. **This wrapper installed:** `.opencode/skills/magic-seed/SKILL.md` (symlink or copy of `platforms/opencode/SKILL.md`).
2. **Universal entry point reachable:** `.ai-workflow/instructions.md`, `.ai-workflow/universal/`, and `.ai-workflow/profiles/{detected}/` — either as files (Layout A) or via a symlink at `.ai-workflow/` pointing at the magic-seed clone (Layout B).
3. **Rendered wizards:** `.ai-workflow/wizards/*.md` and (if needed) `.ai-workflow/rules.md`.
4. **Knowledge base scaffold:** `docs/project/`, `docs/team/`, `docs/features/` per `universal/knowledge-base-spec.md`.

If any of these is missing at the end of init, the install is broken and OpenCode will not be able to re-enter the workflow on the next session. Run `instructions.md` Step 8 (Verify Install) before reporting success.
