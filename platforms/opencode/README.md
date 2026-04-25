# OpenCode wrapper

Install magic-seed for use with [OpenCode](https://opencode.ai).

## Install

OpenCode supports both project-level and global skills. Pick the install path that matches your scope.

### Project-level (recommended)

Use this when magic-seed should be available only inside one repository.

From your project root:

```bash
git clone https://github.com/yourusername/magic-seed.git .ai-workflow
mkdir -p .opencode/skills/magic-seed
ln -s ../../../.ai-workflow/platforms/opencode/SKILL.md .opencode/skills/magic-seed/SKILL.md
```

The symlink lets `git pull` in `.ai-workflow/` update the wrapper automatically. If your environment doesn't support symlinks, copy instead:

```bash
cp .ai-workflow/platforms/opencode/SKILL.md .opencode/skills/magic-seed/SKILL.md
```

### Global

Use this when you want magic-seed available across every project, with the magic-seed core cloned somewhere stable (e.g. `~/tools/magic-seed/`).

```bash
git clone https://github.com/yourusername/magic-seed.git ~/tools/magic-seed
mkdir -p ~/.config/opencode/skills/magic-seed
ln -s ~/tools/magic-seed/platforms/opencode/SKILL.md ~/.config/opencode/skills/magic-seed/SKILL.md
```

You'll still need a per-project `.ai-workflow/` (or equivalent) holding `instructions.md`, `universal/`, and `profiles/` for the AI to read. The simplest pattern is to clone magic-seed once globally and symlink `.ai-workflow` into each project:

```bash
cd your-project
ln -s ~/tools/magic-seed .ai-workflow
```

## How it works

OpenCode reads `SKILL.md` files under skill directories and uses the YAML frontmatter to decide when each skill applies. magic-seed uses the standard fields:

- `name: magic-seed` — the skill identifier
- `description` — OpenCode's agent reads this and loads the skill when the developer's request matches
- `license`, `metadata` — informational

When loaded, the skill points OpenCode at `instructions.md` for the full workflow logic.

OpenCode also reads `.claude/skills/` and `.agents/skills/` for cross-tool compatibility, but the canonical install location for the OpenCode-native wrapper is `.opencode/skills/magic-seed/SKILL.md`.

## Update

```bash
cd .ai-workflow && git pull            # project-level install
cd ~/tools/magic-seed && git pull       # global install
```

The symlink picks up any wrapper changes automatically.

## Uninstall

```bash
# project-level
rm -rf .opencode/skills/magic-seed
rm -rf .ai-workflow

# global
rm -rf ~/.config/opencode/skills/magic-seed
# (keep ~/tools/magic-seed if other projects symlink to it)
```
