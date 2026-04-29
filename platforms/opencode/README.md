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

OpenCode reads `SKILL.md` files under skill directories and exposes each as a tool the agent can call. The YAML frontmatter `description` is what the agent sees when deciding whether to invoke the skill — but **invocation is pull, not push**: the agent decides whether to call `skill({ name: "magic-seed" })` to load the body. There's no auto-activation purely from a description match.

This means installing `.opencode/skills/magic-seed/SKILL.md` alone is **not enough** to make magic-seed work. Without project-level pressure, the agent may improvise feature work instead of loading the skill. Step 7.6 of `instructions.md` covers this by also writing a magic-seed directive to `AGENTS.md` (which OpenCode reads as default project context every session). When init is done correctly, both artifacts are in place.

magic-seed uses the standard skill frontmatter fields:

- `name: magic-seed` — the skill identifier (becomes tool `skills_magic_seed`)
- `description` — what the agent sees in the tool list; ours is imperative and lists trigger phrases
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
