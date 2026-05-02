# Flow: Design

> **Profile:** Godot Game

## When to Use
Starting new tasks in Godot.

## Output
```
flow-storage/tasks/{task-name}/
├── design/
│   ├── task-design.md
│   ├── task-technical-design.md
│   ├── task-edge-cases.md
│   └── diagrams/
└── implement/
    └── flow-plan/
        ├── task-flow-01-{descriptive-name}.md   # Dynamically named (e.g., player-movement)
        ├── task-flow-02-{descriptive-name}.md
        └── ...                                  # AI proposes based on task analysis
```

## Phases

This profile follows the canonical **2-phase, 2-gate** structure from `universal/workflow-structure.md` (and `profiles/generic/skeletons/design-flow.md`). Godot-specific details are in the sub-tasks.

### Phase 1: SPECIFY *(STANDARD gate)*
- **1.1 UNDERSTAND** — Load project context, explore codebase (scenes/, scripts/, autoloads).
- **1.2 DESIGN (Visual-First)** — Generate node hierarchy diagram + script architecture (class + package + sequence as needed). Render every `.puml` to `.svg`.
- **1.3 SPECIFY** — Create `task-design.md`, `task-technical-design.md`, `task-edge-cases.md` with **Godot specifics**:
  - Node tree structure
  - Script attachments
  - Signal connections
  - Scene instancing
- → **Gate 1: Design package review** — diagrams + spec docs together.

### Phase 2: COMMIT *(CRITICAL gate)*
- **2.1 PLAN** — Decompose into implementation task flows (kebab-case names):
  - Examples: `player-movement.md`, `enemy-spawner.md`, `ui-health-bar.md`
  - Create files with frontmatter + tags
  - Generate task-flow dependency diagram
  - Suggested: offer to add `tests.md` as optional task flow
- **2.2 FINALIZE** — Sign off task-design.md (immutable per Rule 9).
- **2.3 COMMIT** *(executes only after the gate is accepted)* — Stage `flow-storage/tasks/{task-name}/`; commit `design({task-name}): sign off task-design.md and {N} task flows`. Skip 2.3 if `--no-commit`.
- → **Gate 2: Plan + final approval** — presented BEFORE 2.3 executes git commit.

## Godot Patterns

```gdscript
# Node with signals
extends CharacterBody2D

signal health_changed(new_health)

@export var max_health: int = 100
var current_health: int

func _ready():
    current_health = max_health

func take_damage(amount: int):
    current_health -= amount
    health_changed.emit(current_health)
```
