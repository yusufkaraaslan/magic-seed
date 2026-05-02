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

### Phase 1: UNDERSTAND

**Purpose:** Load context and understand the task.

**Steps:**
1. Read `flow-storage/project/ARCHITECTURE.md`
2. Read `flow-storage/project/CONVENTIONS.md`
3. Read `flow-storage/project/PATTERNS.md`
4. Search for related features in `flow-storage/tasks/`
5. Explore codebase for similar implementations

**Artifacts:**
- Context summary

**Gate:** Confirm understanding — [A]ccept / [F]eedback / [R]eject

---

### Phase 2: DESIGN (Visual-First)

**Purpose:** Create the design using diagrams as primary interface.

**Steps:**
1. Ask developer: "Describe this task in 1-2 sentences"
2. Generate rough class diagram (PlantUML) showing:
   - Main classes/entities
   - Their relationships
   - Key methods
3. Present diagram to developer
4. Iterate based on feedback:
   - Developer: "Add X", "Remove Y", "Change Z"
   - AI updates diagram
   - Re-present
5. Once approved, generate package/module diagram showing:
   - Where new code fits
   - Dependencies on existing modules
6. Present package diagram
7. Iterate until approved

**Artifacts:**
- `diagrams/01-class-diagram.puml`
- `diagrams/02-package-diagram.puml`

**Gate:** Diagram approval — [A]ccept / [F]eedback / [R]eject

---

### Phase 3: SPECIFY

**Purpose:** Generate detailed specifications from approved diagrams.

**Steps:**
1. Create `task-design.md` from class diagram:
   - Overview
   - Goals and Non-Goals
   - Architecture (referencing diagrams)
   - Data Model
   - API/Interface definitions
   - User Stories
   - Edge Cases (high-level)
   - Open Questions
2. Create `task-technical-design.md` with technical details:
   - Implementation approach
   - File structure
   - Interface contracts
   - Error handling
   - Testing strategy
3. Create `task-edge-cases.md`:
   - Boundary conditions
   - Error scenarios
   - Performance considerations
   - Security considerations
4. If task has complex flows, generate sequence diagram:
   - `diagrams/03-sequence-{flow}.puml`

**Artifacts:**
- `task-design.md`
- `task-technical-design.md`
- `task-edge-cases.md`
- `diagrams/03-sequence-{flow}.puml` (if needed)

**Gate:** Specification review — [A]ccept / [F]eedback / [R]eject

---

### Phase 4: PLAN

**Purpose:** Create implementation task flows based on task analysis.

**Steps:**
1. **AI analyzes task** and proposes issue breakdown:
   - Names describe the WORK, not architectural layers (kebab-case)
   - NUMBER of task flows is flexible (2-10+ depending on task complexity)
   - Examples:
     - Simple task: `data-model.md`, `component.md`, `tests.md` (3 task flows)
     - Complex task: `user-session-store.md`, `auth-service.md`, `login-form-component.md`, `api-error-handling.md`, `rate-limiter.md`, `tests.md` (6 task flows)
   
2. **Present proposal** to developer with rationale:
   ```
   Proposed 4 task flows for "User Authentication" (medium complexity):
   
   1. user-session-store.md       # User session data structures
   2. auth-service.md             # Login/logout business logic
   3. login-form-component.md     # UI form + validation
   4. api-error-handling.md       # Error responses + logging
   
   Suggested: Add tests.md? [Y]/[N]
   
   [A]ccept / [M]odify / [R]edefine
   ```

3. **Developer adjusts:**
   - Add task flows: "Also need rate-limiter.md"
   - Remove task flows: "Skip api-error-handling for now"
   - Rename: "call it password-reset-flow.md"

4. **Create task flow files** with frontmatter:
   ```yaml
   ---
   issue: user-session-store
   task: "user-authentication"
   status: pending
   depends-on: []
   accepted-date: null
   tags: ["backend", "data-model"]
   ---
   ```

5. **Define acceptance criteria** per issue

6. **Create issue dependency diagram:**
   - `diagrams/04-task-flow-dependencies.puml`

7. **Present complete plan**

**Artifacts:**
- Dynamically named task flow files (e.g., `flow-plan/task-flow-01-user-session-store.md`)
- `diagrams/04-task-flow-dependencies.puml`

**Gate:** Plan review — [A]ccept / [F]eedback / [R]eject

---

### Phase 5: FINALIZE

**Purpose:** Lock design and hand off to implementation.

**Steps:**
1. Update task-design.md status:
   ```markdown
   > **Status:** v2.0 (SIGNED OFF)
   > **Signed Off By:** {developer}
   > **Date:** {date}
   > **Immutable:** Yes
   ```
2. Present complete design package:
   - All diagrams
   - task-design.md summary
   - Issue list with dependencies
3. Explain handoff to implement-flow

**Artifacts:**
- Locked task-design.md
- Complete task directory

**Gate:** Final approval — [A]ccept / [F]eedback / [R]eject

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

- Add phases (e.g., "Security Review")
- Remove phases (e.g., skip sequence diagrams for simple tasks)
- Change diagram format (Mermaid instead of PlantUML)
- Add issue types (e.g., "Migration", "Config")
