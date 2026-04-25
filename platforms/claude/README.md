# Claude Code wrapper

Install magic-seed for use with [Claude Code](https://claude.com/claude-code).

## Install

From your project root:

```bash
git clone https://github.com/yourusername/magic-seed.git .ai-workflow
ln -s platforms/claude/SKILL.md .ai-workflow/SKILL.md
```

The symlink lets `git pull` in `.ai-workflow/` update the wrapper automatically. If your environment doesn't support symlinks, copy instead:

```bash
cp .ai-workflow/platforms/claude/SKILL.md .ai-workflow/SKILL.md
```

## How it works

Claude Code auto-discovers `.ai-workflow/SKILL.md` and reads its YAML frontmatter. When the developer's request matches the skill's `description`, Claude loads the file. The wrapper then:

1. Translates slash commands (`/magic-seed init`, `/design-wizard auth`, …) into magic-seed intents.
2. Points Claude at `instructions.md` (in the same directory) for the full workflow logic.

All workflow logic stays in `instructions.md` — the wrapper is intentionally thin so the same core logic works on every platform.

## Update

```bash
cd .ai-workflow && git pull
```

The symlink picks up any wrapper changes automatically.

## Uninstall

```bash
rm -rf .ai-workflow
```

This removes magic-seed but leaves the project's own `docs/` (your knowledge base) untouched.
