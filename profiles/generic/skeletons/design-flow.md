# Flow: Design

> **Profile:** Generic  
> **Purpose:** Design any task, system change, or update with visual diagrams

---

## When to Use

- Starting a new task
- Modifying existing functionality
- Refactoring or architecture changes
- Any work that needs planning before implementation

## Input

- Task idea or requirement (from developer)
- Existing project context (from knowledge base)

## Output

```
flow-storage/tasks/{task-name}/
├── design/
│   ├── task-design.md
│   ├── task-technical-design.md
│   ├── task-edge-cases.md
│   └── diagrams/
│       ├── 01-class-diagram.puml
│       ├── 02-package-diagram.puml
│       ├── 03-sequence-{flow}.puml (if needed)
│       └── 04-task-flow-dependencies.puml
└── implement/
    └── flow-plan/
        ├── task-flow-01-{descriptive-name}.md   # AI proposes based on task analysis
        ├── task-flow-02-{descriptive-name}.md
        └── ...                                  # Variable number (2-10+ depending on complexity)
```

## Prerequisites

- Project context loaded (`flow-storage/project/ARCHITECTURE.md`)
- Developer can describe the task

---

## Phases

This flow has **2 phases, 2 gates**. Each phase contains multiple sub-tasks that auto-proceed without intermediate gates. The gate is presented only when all sub-tasks of a phase complete. See `universal/workflow-structure.md` for the canonical pattern.

---

### Phase 1: SPECIFY

**Purpose:** Load context, design visually, and write the spec docs as one reviewable package.

**Sub-tasks (auto-proceed):**

**1.1 UNDERSTAND**
- Read `flow-storage/project/ARCHITECTURE.md`
- Read `flow-storage/project/CONVENTIONS.md`
- Read `flow-storage/project/PATTERNS.md`
- Search for related work in `flow-storage/tasks/`
- Explore codebase for similar implementations

**1.2 DESIGN (Visual-First)**
- Ask developer: "Describe this task in 1-2 sentences"
- Generate class diagram (PlantUML) showing main entities, relationships, key methods
- Generate package/module diagram showing where new code fits + dependencies
- Render every `.puml` to `.svg` (Rule 4 — diagrams mandatory means *visible* diagrams)
- If no renderer available locally, STOP — install per `universal/diagram-standards.md`, then resume

**1.3 SPECIFY**
- Create `task-design.md`: Overview, Goals/Non-Goals, Architecture (refs diagrams), Data Model, API, User Stories, Edge Cases, Open Questions
- Create `task-technical-design.md`: implementation approach, file structure, interface contracts, error handling, testing strategy
- Create `task-edge-cases.md`: boundary conditions, error scenarios, performance, security
- If task has complex flows, generate sequence diagram `diagrams/03-sequence-{flow}.puml` (rendered)

**Artifacts (presented at gate):**
- `design/task-design.md`
- `design/task-technical-design.md`
- `design/task-edge-cases.md`
- `design/diagrams/01-class-diagram.puml` + `.svg`
- `design/diagrams/02-package-diagram.puml` + `.svg`
- `design/diagrams/03-sequence-{flow}.puml` + `.svg` (if needed)

**Gate:** Design package review — [A]ccept / [F]eedback / [R]eject  
*(GATE TYPE B — STANDARD)*

---

### Phase 2: COMMIT

**Purpose:** Decompose into work items, sign off the design as immutable, and commit the whole package to git as one reviewable unit.

**Sub-tasks (auto-proceed):**

**2.1 PLAN**
- AI analyzes task and proposes task flow breakdown:
  - Names describe the WORK, not architectural layers (kebab-case)
  - NUMBER of task flows is flexible (2-10+ depending on complexity)
  - Examples:
    - Simple task: `data-model.md`, `component.md`, `tests.md` (3 task flows)
    - Complex task: `user-session-store.md`, `auth-service.md`, `login-form-component.md`, `api-error-handling.md`, `rate-limiter.md`, `tests.md` (6 task flows)
- Create task flow files in `implement/flow-plan/` with frontmatter:
  ```yaml
  ---
  task-flow: user-session-store
  task: "user-authentication"
  status: pending
  depends-on: []
  accepted-date: null
  tags: ["backend", "data-model"]
  ---
  ```
- Define acceptance criteria per task flow
- Generate `design/diagrams/04-task-flow-dependencies.puml` + render to `.svg`
- Run internal PLAN validation (surface failures at the Phase 2 gate):
  - Temporal contradictions in deps
  - Undefined types/symbols
  - Coverage gaps
  - Parallel task flows sharing files
  - Oversized task flows (>1 day)

**2.2 FINALIZE**
- Update task-design.md status header:
  ```markdown
  > **Status:** v2.0 (SIGNED OFF)
  > **Signed Off By:** {developer}
  > **Date:** {date}
  > **Immutable:** Yes
  ```

**2.3 COMMIT** *(executes only after Phase 2 gate is accepted)*
- Stage `flow-storage/tasks/{task-name}/` entirely
- Compose commit message:
  ```
  design({task-name}): sign off task-design.md and {N} task flows

  {one-line summary of task scope}

  Task flows created: {list, comma-separated}
  Diagrams: class, package, sequence (and any others)
  ```
- Run `git commit`

**Artifacts (presented at gate):**
- Task flow files in `implement/flow-plan/task-flow-{NN}-{name}.md`
- `design/diagrams/04-task-flow-dependencies.puml` + `.svg`
- task-design.md with sign-off header (not yet committed)
- Staged file list (`git diff --cached --stat`)
- Proposed commit message

**Gate:** Plan + final approval — [A]ccept / [F]eedback / [R]eject  
*(GATE TYPE C — CRITICAL)*

⚠️ Accepting locks task-design.md immutable per Rule 9 AND lands the commit in history. After this point, any change to task-design.md goes in task-technical-design.md instead.

**Failure modes to verify before [A]ccept:**
- PLAN integrity (any internal check failures from sub-task 2.1 still unresolved?)
- Anything you'd want to change in task-design.md? Now is the only time.
- Diagrams cross-referenced correctly from task-design.md?
- All diagrams rendered: every `.puml` has a sibling `.svg` or `.png`
- All task flows from 2.1 actually written to disk?
- Files staged: only `flow-storage/tasks/{task-name}/` contents — no stray edits?
- Commit message: task name correct? Task flow count matches `flow-plan/`? Summary captures *why* not *what*?

**Opt-out:** `--no-commit` skips sub-task 2.3 (gate still presents; PLAN + FINALIZE still happen). Use when bundling design with adjacent work into one commit.

---

## Visual-First Design Protocol

```
Developer: "I want a user authentication task"

AI: Generating class diagram v1...
    [diagram: User, AuthService, TokenManager, UserRepository]

AI: Does this match your intent?
    [A]ccept / [M]odify / [R]estart

Developer: M
Developer: "Add PasswordValidator and EmailService"

AI: Updated diagram v2...
    [diagram with added classes]

AI: [A]ccept / [M]odify / [R]estart

Developer: A

AI: Generating text documentation from approved diagram...
```

---

## Customization

Users can customize this flow by editing the generated copy in `.ai-workflow/flows/`:

- Add sub-tasks within a phase (e.g., "Security Review" inside Phase 1)
- Add a third phase if a new gate is genuinely needed (e.g., a separate "Security sign-off" gate before the COMMIT gate)
- Remove sub-tasks (e.g., skip sequence diagrams for simple tasks)
- Change diagram format (Mermaid instead of PlantUML)
- Add task flow types (e.g., "Migration", "Config")

⚠️ Avoid promoting sub-tasks to top-level phases without a clear reason — each new phase is a new user-facing gate. The 2-gate default is deliberate.
