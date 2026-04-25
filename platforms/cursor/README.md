# Cursor wrapper

Install magic-seed for use with [Cursor](https://cursor.com).

## Install

From your project root:

```bash
git clone https://github.com/yourusername/magic-seed.git .ai-workflow
mkdir -p .cursor/rules
ln -s ../../.ai-workflow/platforms/cursor/magic-seed.mdc .cursor/rules/magic-seed.mdc
```

The symlink lets `git pull` in `.ai-workflow/` update the wrapper automatically. If your environment doesn't support symlinks, copy instead:

```bash
cp .ai-workflow/platforms/cursor/magic-seed.mdc .cursor/rules/magic-seed.mdc
```

## How it works

Cursor reads `.cursor/rules/*.mdc` files and uses the YAML frontmatter to decide when each rule applies. magic-seed uses the **Apply Intelligently** rule type:

- `description` — Cursor's agent reads this and activates the rule when the developer's request matches.
- `alwaysApply: false` — the rule is not loaded into every chat; only when relevant.

When activated, the rule points Cursor at `instructions.md` (in the cloned `.ai-workflow/` directory) for the full workflow logic.

### Note on the legacy `.cursorrules` file

Older Cursor installs used a single `.cursorrules` file at the project root. The current convention is one or more `.mdc` files under `.cursor/rules/`. If your project still has a `.cursorrules` file, you can keep it for unrelated guidance — magic-seed only adds `.cursor/rules/magic-seed.mdc`.

## Update

```bash
cd .ai-workflow && git pull
```

The symlink picks up any wrapper changes automatically.

## Uninstall

```bash
rm .cursor/rules/magic-seed.mdc
rm -rf .ai-workflow
```

This removes magic-seed but leaves the project's own `docs/` (your knowledge base) and any other `.cursor/rules/` files untouched.
