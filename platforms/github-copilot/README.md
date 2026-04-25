# GitHub Copilot wrapper

Install magic-seed for use with [GitHub Copilot](https://github.com/features/copilot).

## Install

From your project root:

```bash
git clone https://github.com/yourusername/magic-seed.git .ai-workflow
mkdir -p .github
ln -s ../.ai-workflow/platforms/github-copilot/copilot-instructions.md .github/copilot-instructions.md
```

The symlink lets `git pull` in `.ai-workflow/` update the wrapper automatically. If your environment doesn't support symlinks, copy instead:

```bash
cp .ai-workflow/platforms/github-copilot/copilot-instructions.md .github/copilot-instructions.md
```

## Already have a `copilot-instructions.md`?

Append the magic-seed wrapper's content to your existing file rather than overwriting it. Order matters: put magic-seed last so it doesn't dominate the context.

```bash
echo "" >> .github/copilot-instructions.md
cat .ai-workflow/platforms/github-copilot/copilot-instructions.md >> .github/copilot-instructions.md
```

## How it works

GitHub Copilot reads `.github/copilot-instructions.md` and prepends its content to every Copilot Chat request and every code-completion context for this repository. There is no triggering mechanism — the file is **always loaded**.

That makes context discipline critical. The magic-seed wrapper is intentionally minimal (~20 lines): it tells Copilot that magic-seed exists, lists the intents that should trigger workflow behavior, and points at `instructions.md` for the full logic. Everyday code completion isn't slowed down.

When the developer's request matches a magic-seed intent, Copilot reads `instructions.md` (and the universal/profile/skeleton files it references) and executes the workflow.

## Update

```bash
cd .ai-workflow && git pull
```

The symlink picks up any wrapper changes automatically.

## Uninstall

```bash
rm .github/copilot-instructions.md
rm -rf .ai-workflow
```

If you appended magic-seed to an existing `copilot-instructions.md`, manually delete the magic-seed section instead of removing the whole file.
