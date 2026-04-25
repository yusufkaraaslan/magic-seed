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

## Why this is brief

GitHub Copilot loads `copilot-instructions.md` into every completion's context. Keeping this file minimal avoids bloating the context for everyday code completion. The full magic-seed workflow lives in `instructions.md`, which is loaded only when relevant.
