# Architecture Explained

Why magic-seed is built this way.

---

## Core Principle: AI as Runtime

Traditional tools build an engine that interprets configuration. magic-seed flips this:

**The AI (Claude, Cursor, etc.) IS the runtime.**

Everything is markdown instructions that the AI reads and follows. No Python engine. No YAML parser. No CLI binary.

### Why This Works

1. **Natural language is flexible.** The AI interprets intent, not syntax.
2. **No maintenance burden.** No engine to update, no DSL to version.
3. **Universal compatibility.** Works with any AI assistant that reads markdown.
4. **Human-readable.** Developers can read and modify the instructions.

### Comparison

| Approach | Engine | Config | Runtime |
|----------|--------|--------|---------|
| Traditional | Python/Node | YAML/JSON | Interpreter |
| magic-seed | None | Markdown | AI Assistant |

---

## Three Layers

```
┌─────────────────────────────────────┐
│         User Project                │
│  .ai-workflow/wizards/*.md         │
│  docs/project/*.md                  │
│  docs/features/*/*.md               │
├─────────────────────────────────────┤
│         Profile Layer               │
│  profiles/{type}/                   │
│    README.md, discovery.md          │
│    rules.md, skeletons/*.md         │
├─────────────────────────────────────┤
│         Universal Layer             │
│  universal/                         │
│    rules.md, workflow-structure.md  │
│    diagram-standards.md, etc.       │
└─────────────────────────────────────┘
```

### Universal Layer

**Shared across ALL projects.**

Contains:
- Non-negotiable rules (documentation first, no auto-approval)
- Workflow structure (phases, gates)
- Diagram standards
- Knowledge base specification

**Why universal?** Some principles apply regardless of tech stack.

### Profile Layer

**One per project type.**

Contains:
- Detection hints (how to identify this project)
- Discovery instructions (how to explore codebase)
- Profile rules (tech-specific conventions)
- Wizard skeletons (templates for wizards)

**Why profiles?** Unity games need different rules than web apps.

### User Project Layer

**Per project.**

Contains:
- Generated wizards (filled with project specifics)
- Knowledge base (designs, issues, patterns)
- Custom rules (team conventions)
- Custom wizards (project-specific)

**Why user layer?** Every project is unique.

---

## How It Works: Execution Flow

```
Developer asks to initialize magic-seed.

AI reads:
  universal/rules.md                    → "Apply these always"
  profiles/*/README.md                  → "Which profile matches?"
  profiles/{matched}/README.md          → "This is a Unity game"
  profiles/{matched}/discovery.md       → "Explore like this..."
  profiles/{matched}/rules.md           → "Follow these rules..."
  profiles/{matched}/skeletons/*.md     → "Fill these templates..."

AI executes discovery:
  → Reads project files
  → Searches for patterns
  → Asks developer questions
  → Fills slot catalog

AI generates wizards:
  → Loads skeletons
  → Fills slots (class names, namespaces, etc.)
  → Applies rules
  → Writes to .ai-workflow/wizards/

Developer uses wizards by asking:
  "Design a feature called my-feature"
    → AI reads the generated design-wizard.md in .ai-workflow/wizards/
    → Follows instructions
    → Presents for review at each phase
```

---

## Why Markdown?

### Version Control

Markdown files are:
- Text-based (diff-friendly)
- Human-readable
- Reviewable in PRs
- Mergeable

### Portability

Markdown files:
- Work on any platform
- Don't require compilation
- Can be read by any text editor
- Render nicely in GitHub/GitLab

### Simplicity

No need for:
- Schema definitions
- Type systems
- Build steps
- Runtime dependencies

---

## Why No Inheritance?

Profiles are **independent** (no inheritance chain).

**Why?**
1. **Simplicity** — No complex inheritance rules to learn
2. **Self-contained** — Each profile is complete
3. **Easy to understand** — Read one file, understand the profile
4. **Easy to modify** — Change one profile without side effects

**Trade-off:** Some duplication between profiles. Acceptable for simplicity.

---

## Why Visual-First Design?

Diagrams come BEFORE text in design-wizard.

**Why?**
1. **Faster understanding** — Humans process diagrams faster
2. **Early feedback** — Catch misunderstandings before code
3. **Explicit relationships** — Class dependencies are visible
4. **Universal language** — Diagrams work across languages

---

## Why Living Documentation?

Docs are updated at every step, never archived.

**Why?**
1. **Accurate** — Always reflects current code
2. **Discoverable** — New devs find current info
3. **Searchable** — Knowledge base grows
4. **Valuable** — Becomes project asset

---

## Why Three-Tier Knowledge Base?

```
Feature-level → Project-level → Team-level
```

**Why?**
1. **Feature:** Details of specific work
2. **Project:** Shared patterns and decisions
3. **Team:** Onboarding and processes

Each tier serves different needs and audiences.

---

## Trade-offs

### What magic-seed sacrifices:

- **Speed** — AI interpretation is slower than compiled code
- **Determinism** — AI may interpret instructions differently
- **Validation** — Can't run automated checks without tools
- **IDE integration** — No syntax highlighting for markdown instructions

### What magic-seed gains:

- **Flexibility** — Adapt to any project without code changes
- **Simplicity** — No complex setup or configuration
- **Maintainability** — No engine to version or update
- **Universality** — Works with any AI assistant

---

## Future Directions

Possible enhancements (not in core):

1. **IDE Plugins** — Syntax highlighting for markdown wizards
2. **Validation Tools** — Optional CLI to check markdown validity
3. **Template Marketplace** — Community-contributed profiles
4. **AI Model Tuning** — Fine-tune models on markdown instructions
5. **Diagram Rendering** — Auto-render PlantUML in CI/CD

These are additive, not required. Core remains markdown-only.

---

## Philosophy

**magic-seed believes:**

1. **AI is the future of development tools.** Don't build interpreters, write instructions.
2. **Documentation is code.** Living docs are as important as source code.
3. **Humans decide, AI assists.** Never auto-approve. Developer is always in control.
4. **Simplicity wins.** Fewer moving parts means fewer failures.
5. **Community grows value.** Share profiles, share patterns, share knowledge.

---

## Questions?

- [How to create a profile](how-to-create-profile.md)
- [How to write rules](how-to-write-rules.md)
- [How to add a wizard](how-to-add-wizard.md)
- [How to customize](how-to-customize.md)
- [Quick start](../README.md)
