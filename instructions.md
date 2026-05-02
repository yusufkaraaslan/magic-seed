# magic-seed

> **You are the engine.** Read these markdown files, interpret them, and act according to their instructions. No external tools, no CLI, no DSL. Just markdown.

This file is the platform-neutral entry point for magic-seed. Platform wrappers (in `platforms/{tool}/`) point at this file and translate platform-specific triggers (Claude Code skill descriptions, Cursor `.mdc` rules, GitHub Copilot instructions, OpenCode skills, Kimi Code `AGENTS.md`, etc.) into the intents listed below.

---

## Supported Actions

magic-seed responds to the following developer intents. Each platform wrapper documents how to invoke them on its tool — but the underlying behavior is the same everywhere.

| Intent | What the AI does |
|--------|------------------|
| **Initialize magic-seed for this project** | Detect project type, discover the codebase, ask the developer for missing context, and generate tailored wizards into `.ai-workflow/wizards/`. |
| **Run a specific wizard** (design, implement, pr, test, deploy, docs) | Load the corresponding wizard file from `.ai-workflow/wizards/` and execute its phases against the named feature. |
| **Show project workflow status** | Read the knowledge base and report progress across in-flight features and their issues. |
| **Search the knowledge base** | Query `docs/project/`, `docs/team/`, and `docs/features/` for relevant prior work. |

When the developer expresses one of these intents — whether through a slash command on a platform that supports them, a natural-language request, or any other channel — the AI follows the workflow described below.

---

## How It Works

### Step 0: Identify the Host AI Tool

Before anything else, identify **which AI CLI / IDE the developer is using to invoke magic-seed**. This determines which platform wrapper from `platforms/{tool}/` must be installed in Step 7.

**Do not infer the tool from the model name.** "Kimi" and "Claude" are LLM providers; "OpenCode", "Claude Code", "Cursor", "Kimi-Code CLI", and "GitHub Copilot" are the *tools* — they each have their own discovery convention and install path. A single LLM (e.g. Kimi) can be used inside multiple tools, and each tool needs its own wrapper.

**Detection signals (check in this order):**

| Signal in project | Likely tool | Wrapper to install |
|---|---|---|
| `.opencode/` exists, or developer says "OpenCode" | OpenCode CLI | `platforms/opencode/SKILL.md` → `.opencode/skills/magic-seed/SKILL.md` |
| `.claude/` exists, or developer says "Claude Code" | Claude Code | `platforms/claude/SKILL.md` → `.claude/skills/magic-seed/SKILL.md` |
| `.cursor/` exists, or developer says "Cursor" | Cursor | `platforms/cursor/magic-seed.mdc` → `.cursor/rules/magic-seed.mdc` |
| `.github/copilot-instructions.md` exists, or developer says "Copilot" | GitHub Copilot | `platforms/github-copilot/copilot-instructions.md` → `.github/copilot-instructions.md` |
| Developer explicitly says "Kimi-Code CLI" (the tool, not the model) | Kimi-Code CLI | `platforms/kimi-code/AGENTS.md` → `AGENTS.md` |

**If none of these exist or are mentioned, ask the developer explicitly:**

> "Which AI tool will be invoking magic-seed in this project?
> 1. Claude Code (CLI/IDE)
> 2. OpenCode CLI
> 3. Cursor IDE
> 4. GitHub Copilot
> 5. Kimi-Code CLI
> 6. Other / multiple — install more than one wrapper"

**If multiple tools are in use, install multiple wrappers** — they coexist without conflict, and the universal `instructions.md` + `universal/` + `profiles/` files are shared by all of them.

Record the chosen tool(s) in the slot catalog as `host-tool` (single) or `host-tools` (list).

### Step 1: Detect Project Type

Read all `profiles/*/README.md` files. Each profile contains detection hints. Score the current project against each profile.

**Detection strategy:**
1. **Auto-detect** — Scan project files, match against profile detection hints
2. **Ask developer** — If multiple profiles match with similar confidence, present options
3. **Create custom** — If no profile matches, ask: "Would you like to create a custom profile from scratch?"

**Example detection logic:**
- Unity: `Assets/` + `*.cs` + `ProjectSettings/` = high confidence
- Web: `package.json` + `src/` = high confidence  
- Godot: `project.godot` + `*.gd` = high confidence
- Generic: Any project (fallback, always matches with low confidence)

### Step 2: Load Universal Rules

Always read `universal/rules.md` first. These are non-negotiable guardrails that apply to ALL projects regardless of type.

### Step 3: Load Profile

Read the detected profile:
- `profiles/{type}/README.md` — Profile overview and capabilities
- `profiles/{type}/discovery.md` — Instructions for exploring this project type
- `profiles/{type}/rules.md` — Profile-specific conventions and guardrails
- `profiles/{type}/skeletons/*.md` — Wizard templates to fill

### Step 4: Execute Discovery

Follow `profiles/{type}/discovery.md` instructions. The AI explores the codebase by:
- Reading key files (README, package manifests, config files)
- Searching for patterns (architecture, frameworks, conventions)
- Asking the developer about undiscoverable decisions

Record all findings in a slot catalog format.

### Step 5: Generate Wizards

For each skeleton in `profiles/{type}/skeletons/`:
1. Load the skeleton markdown
2. Fill project-specific slots with discovered values
3. Apply universal rules + profile rules
4. Write to `.ai-workflow/wizards/{wizard-name}.md`

### Step 6: Review with Developer

Present the generated wizards to the developer:
- Show file list and summaries
- Highlight key decisions made during generation
- Offer review options: **[A]ccept / [F]eedback / [R]eject**
- Iterate on feedback until accepted

### Step 7: Install

Init is **not just rendering output** — it must persist enough of magic-seed into the project that the *next* AI session can re-enter the workflow without you re-explaining anything. Generate-and-forget breaks the system: the platform wrapper says "Read `instructions.md` before acting on any magic-seed intent," and that file must actually exist locally for that to work.

Persist the following into the user's project (in roughly this order):

#### 7.1 Platform wrapper (per Step 0 host tool)

For each tool identified in Step 0, install the matching wrapper(s) to its canonical path. See `platforms/{tool}/README.md` for the exact install command. The summary:

| Tool | Source | Destination |
|---|---|---|
| OpenCode | `platforms/opencode/SKILL.md` (orchestrator) **plus** all six wizard skill bodies in `platforms/opencode/wizard-skills/{wizard}/SKILL.md` **plus** all six command files in `platforms/opencode/commands/{name}.md` | `.opencode/skills/magic-seed/SKILL.md`, `.opencode/skills/{design,implement,pr,test,deploy,docs}-wizard/SKILL.md`, AND `.opencode/commands/{design,implement,test,pr,deploy,docs}.md` (7 skills + 6 commands = 13 files) |
| Claude Code | `platforms/claude/SKILL.md` | `.claude/skills/magic-seed/SKILL.md` |
| Cursor | `platforms/cursor/magic-seed.mdc` | `.cursor/rules/magic-seed.mdc` |
| GitHub Copilot | `platforms/github-copilot/copilot-instructions.md` | `.github/copilot-instructions.md` (append, do not overwrite if file already exists) |
| Kimi-Code CLI | `platforms/kimi-code/AGENTS.md` | `AGENTS.md` at repo root (append, do not overwrite — see notes in `platforms/kimi-code/README.md`) |

**Why OpenCode installs seven skills plus six commands:** OpenCode has two concepts — *skills* (`.opencode/skills/{name}/SKILL.md`, agent decides via tool call when to load) and *commands* (`.opencode/commands/{name}.md`, user types `/{name}` to invoke directly). They're complementary, not redundant. Skills give the agent strong description-driven matching for natural-language requests; commands give the developer a one-keystroke way to start a wizard without going through the `/skills` picker. Installing only skills leaves the developer with a two-click invocation (`/skills` → pick); installing both gets parity with Claude Code's `/design-wizard <feature>` slash UX. Each command file (~15–25 lines) is a prompt template that loads the matching wizard skill and passes `$ARGUMENTS` as the feature/issue.

Prefer **symlinks** over copies if the OS supports them, so `git pull` in `.ai-workflow/` propagates wrapper updates automatically. Fall back to copy when symlinks aren't available.

#### 7.2 Universal entry point + universal/ + detected profile

The wrapper points at `instructions.md`, and `instructions.md` references `universal/rules.md` etc. — these files must be reachable from the project. Two valid layouts:

**Layout A — project-level install (self-contained, recommended for most teams):**

Clone or copy the magic-seed core into `.ai-workflow/` so the project owns its own copy:

```
.ai-workflow/
├── instructions.md          ← from magic-seed root
├── universal/               ← copy of universal/ from magic-seed
├── profiles/{detected}/     ← at minimum, the detected profile
├── wizards/                 ← rendered output (Step 5)
└── rules.md                 ← project-specific overrides (Step 7.4)
```

**Layout B — global install + symlink (only viable when the magic-seed clone is project-private):**

Magic-seed lives once at e.g. `~/tools/magic-seed/`. The project's `.ai-workflow/` symlinks to it:

```bash
ln -s ~/tools/magic-seed .ai-workflow
# then write .ai-workflow/wizards/*.md and .ai-workflow/rules.md
```

⚠️ **Caveat:** the rendered `.ai-workflow/wizards/` and `.ai-workflow/rules.md` are written *into* the global magic-seed clone (since `.ai-workflow/` resolves to that clone). They become uncommitted additions to that clone's working tree, which collides with `git pull` updates and conflates project-specific output with magic-seed core.

This is only safe if **one project owns the magic-seed clone** (e.g. `~/projects/myapp/.magic-seed/` symlinked from `~/projects/myapp/.ai-workflow/`). It does *not* work when multiple projects share `~/tools/magic-seed/` — they'd all write their wizards into the same place.

For shared global clones, prefer **Layout A** instead — copy the relevant subset into the project, accept the drift cost, and `git pull` the global clone separately when wrappers/rules update.

If unsure, use Layout A. It survives moves, doesn't depend on a global path, and keeps project output cleanly separated from magic-seed core.

#### 7.3 Rendered wizards

Write the wizards generated in Step 5 to `.ai-workflow/wizards/{wizard-name}.md`. These are project-specific renders of the profile skeletons.

#### 7.4 Project-specific rules

If the discovery step turned up project-specific rules that don't belong in the universal or profile layer (e.g. "exactly 5 autoloads" for a specific game), write them to `.ai-workflow/rules.md` with severity tags. These layer on top of `universal/rules.md` and the profile's `rules.md` at runtime.

#### 7.5 Knowledge base scaffolding

Create the initial knowledge base structure per `universal/knowledge-base-spec.md`:

```
docs/
├── project/
│   ├── ARCHITECTURE.md
│   ├── CONVENTIONS.md
│   ├── PATTERNS.md
│   └── DECISIONS.md
├── team/
│   ├── onboarding.md
│   └── workflows.md
└── features/                ← empty, populated by design-wizard
```

Populate `project/*.md` from discovery findings (architecture, naming conventions, ADRs surfaced from existing docs). Leave `features/` empty.

#### 7.6 Project-level directive (AGENTS.md / CLAUDE.md augmentation)

**Why this step exists:** Most AI tools read project-root files (`AGENTS.md`, `CLAUDE.md`) as default session context, *separately* from the platform skill / wrapper file they may also discover. A platform wrapper alone is not enough — for tools where skills are pull-not-push (OpenCode invokes skills via a `skill` tool call the agent decides whether to make), the agent has no project-level pressure to defer to magic-seed and may just improvise on natural-language requests like "design feature X". The fix is to put a directive in the file the tool *always* reads.

**Action:** If `AGENTS.md` or `CLAUDE.md` exists at the project root (or both), append a magic-seed directive section to each. Skip this step for the Kimi-Code CLI install path — there `AGENTS.md` *is* the magic-seed wrapper, the directive is implicit. If neither file exists and the host tool is one that reads them (OpenCode reads `AGENTS.md`, Claude Code reads `CLAUDE.md`), create the file with just the directive.

**Idempotency:** Use a sentinel comment so re-runs don't duplicate. Wrap the directive in:

```markdown
<!-- magic-seed-directive-start -->
... directive content ...
<!-- magic-seed-directive-end -->
```

On re-init, replace the content between the sentinels rather than appending again.

**Directive content** (paste this verbatim, then customize the rendered wizard table if the project added custom wizards):

```markdown
<!-- magic-seed-directive-start -->
## magic-seed Workflow (Mandatory)

This project uses **magic-seed** for feature development. Before writing any code for a feature, you MUST run the design-wizard and obtain a signed-off `docs/features/{name}/DESIGN.md`.

When the developer's request matches one of the intents below, load and follow the corresponding wizard from `.ai-workflow/wizards/`. On platforms where wizards are exposed as a skill/tool (OpenCode `skill` tool, Claude Code skill, etc.), invoke that mechanism to load magic-seed before acting.

| Developer says | Wizard |
|---|---|
| "design feature X" / "let's design X" | `.ai-workflow/wizards/design-wizard.md` |
| "implement X" / "next issue" | `.ai-workflow/wizards/implement-wizard.md` |
| "test X" / "add tests" | `.ai-workflow/wizards/test-wizard.md` |
| "validate X for PR" / "PR check" | `.ai-workflow/wizards/pr-wizard.md` |
| "deploy X" / "build" | `.ai-workflow/wizards/deploy-wizard.md` |
| "update docs" / "docs for X" | `.ai-workflow/wizards/docs-wizard.md` |

**Hard rules — do not violate:**

- No feature code without a signed-off `DESIGN.md`. If the developer asks to implement a feature with no prior design, run **design-wizard first** even if they didn't ask for it.
- Every wizard phase ends with `[A]ccept / [F]eedback / [R]eject`. Never auto-accept. Present the gate to the developer.
- Universal rules in `.ai-workflow/universal/rules.md` are non-negotiable. Profile rules and `.ai-workflow/rules.md` layer on top.

For the full workflow contract, read `.ai-workflow/instructions.md`.
<!-- magic-seed-directive-end -->
```

**Where this directive lands per host tool:**

| Host tool (Step 0) | Directive target | Notes |
|---|---|---|
| OpenCode CLI | `AGENTS.md` | OpenCode reads `AGENTS.md` as default context. Append/update the sentinel block. |
| Claude Code | `CLAUDE.md` | Append/update the sentinel block. The Claude SKILL.md command-alias table already gives the agent slash invocations, but the directive in `CLAUDE.md` ensures the agent defers even when the developer uses natural language. |
| Cursor | `AGENTS.md` *if it exists* | Cursor's `.mdc` rules drive primary behavior; the AGENTS.md directive is belt-and-suspenders for cross-tool consistency. |
| GitHub Copilot | n/a | The Copilot wrapper *is* `.github/copilot-instructions.md` and is always loaded; no separate AGENTS.md needed. |
| Kimi-Code CLI | n/a | Skip — `AGENTS.md` is the wrapper itself. |

If multiple host tools were chosen in Step 0, write the directive to every applicable target.

---

### Step 8: Verify Install

Before declaring init complete, confirm every artifact landed. The AI must be able to answer "yes" to all of these:

- [ ] **Wrapper(s) installed** — the file(s) at the destination(s) from the table in Step 7.1 exist and (if symlinks) resolve to the magic-seed source. For OpenCode specifically, verify all **seven** skills are present: `.opencode/skills/{magic-seed,design-wizard,implement-wizard,pr-wizard,test-wizard,deploy-wizard,docs-wizard}/SKILL.md`, AND all **six** commands: `.opencode/commands/{design,implement,test,pr,deploy,docs}.md`. The skills picker should show 7 entries; typing `/` should suggest the 6 commands. Either missing means the install is incomplete.
- [ ] **`instructions.md` reachable** — either at `.ai-workflow/instructions.md` (Layout A) or via a symlink resolving to magic-seed (Layout B). The wrapper's "Read `instructions.md` before acting" sentence must not be a dangling reference.
- [ ] **`universal/rules.md` reachable** — at `.ai-workflow/universal/rules.md` or via the symlink.
- [ ] **Detected profile reachable** — at `.ai-workflow/profiles/{detected}/` or via the symlink. At minimum the four files `README.md`, `discovery.md`, `rules.md`, and `skeletons/` must be present.
- [ ] **Wizards rendered** — `.ai-workflow/wizards/*.md` exists, no leaked `{slot}` placeholders (Rule 12), phase gates `[A]/[F]/[R]` intact.
- [ ] **Knowledge base scaffold** — `docs/project/`, `docs/team/`, `docs/features/` exist with the expected files.
- [ ] **Project directive present** — for OpenCode, Claude Code, or Cursor host tools, the matching root file (`AGENTS.md` and/or `CLAUDE.md`) contains the magic-seed directive between `<!-- magic-seed-directive-start -->` and `<!-- magic-seed-directive-end -->` sentinels. Skip for Copilot and Kimi-Code (their wrappers serve this role). Without this, the agent has no project-level pressure to defer to wizards on natural-language requests and may improvise feature work — making the install functionally inert even though all files are in place.
- [ ] **Smoke test** — re-read the wrapper file you just installed and confirm the path it references (`instructions.md`) resolves. If it doesn't, the install is broken.

If any check fails, fix it before reporting "magic-seed initialized" to the developer. Do not present the install as successful when the next session won't be able to load the workflow.

---

## File Reading Order

When acting on any magic-seed intent, read files in this order. Paths are relative to the magic-seed root, which after install is reachable in the user's project via either `.ai-workflow/` (Layout A) or a symlink at `.ai-workflow/` (Layout B). For a fresh init *before* install, read directly from the magic-seed checkout the wrapper points at.

```
1. universal/rules.md                    (always)
2. profiles/{detected}/README.md         (after detection)
3. profiles/{detected}/discovery.md      (after detection)
4. profiles/{detected}/rules.md          (after detection)
5. profiles/{detected}/skeletons/*.md    (during generation)
6. universal/diagram-standards.md        (when generating diagrams)
7. universal/knowledge-base-spec.md      (when creating KB)
8. .ai-workflow/rules.md                 (if present — project-specific overrides, layered last)
```

---

## Project Detection Reference

### Unity Game
**Indicators:**
- `Assets/` directory exists
- `*.cs` files present
- `ProjectSettings/ProjectSettings.asset` exists
- `Packages/manifest.json` exists

**Confidence:**
- 4/4 matches: "This is definitely a Unity project"
- 3/4 matches: "Likely Unity — ask developer to confirm"

### Godot Game
**Indicators:**
- `project.godot` exists
- `*.gd` files present
- `scene/` or `scenes/` directory

### Web Frontend
**Indicators:**
- `package.json` exists
- `src/` directory with `.tsx`, `.jsx`, `.vue`, or `.svelte` files
- `vite.config.*`, `webpack.config.*`, or `next.config.*` exists

### Backend API
**Indicators:**
- `pyproject.toml`, `go.mod`, `Cargo.toml`, `package.json` with server frameworks
- `src/` or `app/` with API route patterns
- `Dockerfile` or `docker-compose.yml`

### Mobile App
**Indicators:**
- Flutter: `pubspec.yaml`, `lib/`, `ios/`, `android/`
- React Native: `package.json`, `ios/`, `android/`, `metro.config.js`

### Generic
**Indicators:**
- Any project with files
- Use as fallback when no other profile matches

---

## Custom Profile Creation

If no profile matches, ask the developer:

> "No built-in profile matches your project. Would you like to:
> 1. Use the **generic** profile (minimal, works for anything)
> 2. **Create a custom profile** from scratch (I'll guide you)
> 3. **Extend an existing profile** (closest match: {best-guess})"

If they choose "Create custom," follow `universal/wizard-authoring-guide.md`.

---

## Knowledge Base Integration

After wizard generation, the knowledge base structure is:

```
docs/
├── project/
│   ├── ARCHITECTURE.md       (created during init)
│   ├── CONVENTIONS.md        (created during init)
│   ├── PATTERNS.md           (populated by wizards)
│   └── DECISIONS.md          (ADRs from design phase)
├── team/
│   ├── onboarding.md         (team-specific)
│   ├── workflows.md          (team-specific)
│   └── glossary.md           (domain terms)
└── features/
    └── {feature-name}/
        ├── DESIGN.md
        ├── TDD.md
        ├── EDGE-CASES.md
        ├── diagrams/
        ├── issues/
        ├── feedback/
        └── lessons-learned.md
```

---

## Wizard Execution

When the developer asks to run a wizard against a feature (for example, "design feature my-feature" or "implement the next issue for my-feature"):

1. Read the corresponding wizard file in `.ai-workflow/wizards/` (e.g., `design-wizard.md`)
2. Follow its instructions step by step
3. Load project context from the knowledge base
4. Execute phases, producing the artifacts the wizard specifies
5. Present for developer review at each phase gate

The same pattern applies to all wizard types — design, implement, pr, test, deploy, docs — and to any custom wizards the project has added.

---

## Customization Points

Users can customize without modifying magic-seed core:

- **`.ai-workflow/rules.md`** — Project-specific rules (merged with universal + profile)
- **`.ai-workflow/wizards/*.md`** — Custom wizards or overrides
- **`docs/project/CONVENTIONS.md`** — Team coding standards
- **`docs/project/PATTERNS.md`** — Reusable patterns discovered by wizards

---

## Important Notes

- **Never skip developer review.** Every phase ends with [A]ccept / [F]eedback / [R]eject.
- **Never auto-accept.** The AI makes suggestions, the developer decides.
- **Unlimited iterations.** No maximum review rounds. Quality over speed.
- **Living documentation.** Docs are updated at every step, never archived.
- **Diagrams are mandatory.** Minimum class diagram + package diagram per feature.
- **Knowledge capture.** After every feature, update lessons-learned and patterns catalog.
