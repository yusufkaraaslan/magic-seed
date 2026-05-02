# OpenCode wrapper

Install magic-seed for use with [OpenCode](https://opencode.ai).

## What gets installed

**7 skills + 6 commands = 13 files.** The skills are for agent auto-invocation (description-driven); the commands are for user-typed slash invocation. They complement each other.

### Skills (7)

| Skill | Source | Role |
|---|---|---|
| `magic-seed` | `platforms/opencode/SKILL.md` | Orchestrator — init, status, knowledge-base search, ambiguous requests |
| `design-wizard` | `platforms/opencode/wizard-skills/design-wizard/SKILL.md` | Phase: design (DESIGN.md, TDD.md, diagrams, issues) |
| `implement-wizard` | `platforms/opencode/wizard-skills/implement-wizard/SKILL.md` | Phase: implement one issue |
| `pr-wizard` | `platforms/opencode/wizard-skills/pr-wizard/SKILL.md` | Phase: PR validation, feedback, post-merge capture |
| `test-wizard` | `platforms/opencode/wizard-skills/test-wizard/SKILL.md` | Phase: tests |
| `deploy-wizard` | `platforms/opencode/wizard-skills/deploy-wizard/SKILL.md` | Phase: build/export/release |
| `docs-wizard` | `platforms/opencode/wizard-skills/docs-wizard/SKILL.md` | Phase: knowledge-base maintenance |

Each wizard skill has an imperative description naming its trigger phrases. The agent uses these descriptions to decide whether to invoke `skill({ name })` when the user makes a natural-language request like "design feature X."

### Commands (6)

| Command | Source | Invocation |
|---|---|---|
| `/design <feature>` | `platforms/opencode/commands/design.md` | Run design-wizard for a feature |
| `/implement <feature> [issue]` | `platforms/opencode/commands/implement.md` | Implement one issue from a designed feature |
| `/test [feature]` | `platforms/opencode/commands/test.md` | Run or add tests |
| `/pr <feature> [mode]` | `platforms/opencode/commands/pr.md` | Pre-PR validation, feedback, or capture |
| `/deploy [feature]` | `platforms/opencode/commands/deploy.md` | Build verification + smoke test |
| `/docs [scope]` | `platforms/opencode/commands/docs.md` | Audit/update knowledge base |

Each command is a prompt template that loads the matching wizard skill and passes `$ARGUMENTS` as the feature/issue. Without these, the developer has to either rely on natural-language matching to the skills (works but unreliable) or use `/skills → pick` (two clicks). With commands, `/design board-system` is one keystroke.

## Install

OpenCode supports both project-level and global skills. Pick the install path that matches your scope.

### Project-level (recommended)

Use this when magic-seed should be available only inside one repository.

From your project root:

```bash
git clone https://github.com/yourusername/magic-seed.git .ai-workflow

# Install 7 skills + 6 commands as symlinks (preferred):
for skill in magic-seed design-wizard implement-wizard pr-wizard test-wizard deploy-wizard docs-wizard; do
  mkdir -p ".opencode/skills/$skill"
  if [ "$skill" = "magic-seed" ]; then
    ln -s "../../../.ai-workflow/platforms/opencode/SKILL.md" ".opencode/skills/$skill/SKILL.md"
  else
    ln -s "../../../.ai-workflow/platforms/opencode/wizard-skills/$skill/SKILL.md" ".opencode/skills/$skill/SKILL.md"
  fi
done

mkdir -p ".opencode/commands"
for cmd in design implement test pr deploy docs; do
  ln -s "../../.ai-workflow/platforms/opencode/commands/$cmd.md" ".opencode/commands/$cmd.md"
done
```

Symlinks let `git pull` in `.ai-workflow/` update every wrapper automatically. If your environment doesn't support symlinks, replace `ln -s` with `cp`:

```bash
for skill in magic-seed design-wizard implement-wizard pr-wizard test-wizard deploy-wizard docs-wizard; do
  mkdir -p ".opencode/skills/$skill"
  if [ "$skill" = "magic-seed" ]; then
    cp ".ai-workflow/platforms/opencode/SKILL.md" ".opencode/skills/$skill/SKILL.md"
  else
    cp ".ai-workflow/platforms/opencode/wizard-skills/$skill/SKILL.md" ".opencode/skills/$skill/SKILL.md"
  fi
done

mkdir -p ".opencode/commands"
for cmd in design implement test pr deploy docs; do
  cp ".ai-workflow/platforms/opencode/commands/$cmd.md" ".opencode/commands/$cmd.md"
done
```

**Caveat for copy installs:** when you `git pull` in `.ai-workflow/`, you must re-run the loop above to refresh the copies. OpenCode's server caches skills in memory at startup — after refreshing, restart the server (not just the TUI) for changes to take effect.

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
