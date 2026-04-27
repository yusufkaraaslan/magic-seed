# magic-seed is installed in this repository

This project uses **magic-seed**, a markdown-only AI workflow generator. The full workflow logic lives in `instructions.md` at the repo root (or under `.ai-workflow/` if installed there).

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

When the developer says "initialize magic-seed for this project", the agent must follow `instructions.md` Step 7 in full. A successful Copilot init produces all four of: this file at `.github/copilot-instructions.md` (appended, not overwriting any existing content); universal entry point reachable at `.ai-workflow/{instructions.md, universal/, profiles/{detected}/}`; rendered wizards at `.ai-workflow/wizards/*.md`; and knowledge base scaffold at `docs/{project,team,features}/`. Run `instructions.md` Step 8 (Verify Install) before reporting success.

## Why this is brief

GitHub Copilot loads `copilot-instructions.md` into every completion's context. Keeping this file minimal avoids bloating the context for everyday code completion. The full magic-seed workflow lives in `instructions.md`, which is loaded only when relevant.
