---
name: magic-seed
description: Universal workflow generator. Detects your project type, asks about your needs, and generates tailored AI-assisted development wizards (design, implement, PR, test, deploy, docs) with living documentation and knowledge base support. Zero installation — everything is markdown that the AI reads and executes.
argument-hint: [command] [args]
---

# magic-seed

> **You are the engine.** Read these markdown files, interpret them, and act according to their instructions. No external tools, no CLI, no DSL. Just markdown.

---

## Supported Commands

| Command | Description |
|---------|-------------|
| `/magic-seed init` | Detect project type, discover codebase, ask developer, generate wizards |
| `/magic-seed wizard <name>` | Run a specific wizard (design, implement, pr, test, deploy, docs) |
| `/magic-seed status` | Show project workflow status |
| `/magic-seed kb <query>` | Search the knowledge base |

---

## How It Works

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

Write accepted wizards to `.ai-workflow/wizards/`.
Create initial knowledge base structure in `docs/`.

---

## File Reading Order

When executing any `/magic-seed` command, read files in this order:

```
1. universal/rules.md                    (always)
2. profiles/{detected}/README.md         (after detection)
3. profiles/{detected}/discovery.md      (after detection)
4. profiles/{detected}/rules.md          (after detection)
5. profiles/{detected}/skeletons/*.md    (during generation)
6. universal/diagram-standards.md        (when generating diagrams)
7. universal/knowledge-base-spec.md      (when creating KB)
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

When developer runs `/design-wizard my-feature`:

1. Read `.ai-workflow/wizards/design-wizard.md`
2. Follow its instructions step by step
3. Load project context from knowledge base
4. Execute phases, produce artifacts
5. Present for developer review at each gate

Same pattern for implement-wizard, pr-wizard, and any custom wizards.

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
