# magic-seed is installed in this repository

This project uses **magic-seed**, a markdown-only AI workflow generator. The full workflow logic lives in `instructions.md` (typically at `.ai-workflow/instructions.md` in this project).

## Tool scope

This `AGENTS.md` is the wrapper for the **Kimi-Code CLI** specifically. If the developer is using a different tool (OpenCode CLI, Claude Code, Cursor, GitHub Copilot) that *also* happens to use Kimi as a model provider, that's a different tool with its own wrapper — see `platforms/{tool}/` in the magic-seed source. The model and the tool are not the same thing.

## When the developer asks about magic-seed

If the developer's request matches one of these intents, read `instructions.md` and follow the workflow it describes. Do not improvise — magic-seed has strict review gates and a defined phase architecture.

- Initialize magic-seed for this project
- Design a feature
- Implement the next issue for a feature
- Run a wizard (design, implement, pr, test, deploy, docs) against a feature
- Validate a feature for PR
- Process PR feedback
- Capture lessons learned for a feature
- Search the project knowledge base

For all other requests, ignore this section.

## Init must persist plumbing, not just generate output

When the developer says "initialize magic-seed for this project", the agent must follow `instructions.md` Step 7 in full. A successful Kimi-Code init produces **all four** of the following, not just the last two:

1. **This file persisted:** `AGENTS.md` at the repo root. If `AGENTS.md` already exists for other reasons, **append** the magic-seed section — do not overwrite.
2. **Universal entry point reachable:** `.ai-workflow/instructions.md`, `.ai-workflow/universal/`, and `.ai-workflow/profiles/{detected}/` — either as files (Layout A) or via a symlink at `.ai-workflow/` pointing at the magic-seed clone (Layout B).
3. **Rendered wizards:** `.ai-workflow/wizards/*.md` and (if needed) `.ai-workflow/rules.md`.
4. **Knowledge base scaffold:** `docs/project/`, `docs/team/`, `docs/features/` per `universal/knowledge-base-spec.md`.

If any of these is missing at the end of init, the install is broken — the next Kimi-Code session won't be able to re-enter the workflow. Run `instructions.md` Step 8 (Verify Install) before reporting success.

## Why this is brief

This file lives at the project root and may be merged into the agent's system prompt context (via `${KIMI_AGENTS_MD}` or similar mechanisms in other tools that support `AGENTS.md`). Keeping it minimal avoids bloating the context for everyday work. The full magic-seed workflow lives in `instructions.md`, which is loaded only when relevant.
