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

### Phase 1: UNDERSTAND
Load project context and explore codebase.

### Phase 2: DESIGN (Visual-First)
Generate node hierarchy diagram and script architecture.

### Phase 3: SPECIFY
Create task-design.md, task-technical-design.md, task-edge-cases.md with Godot specifics:
- Node tree structure
- Script attachments
- Signal connections
- Scene instancing

### Phase 4: PLAN
**Purpose:** Create implementation task flows based on task analysis.

**Process:**
1. AI analyzes task and proposes task flows (kebab-case names):
   - Examples: `player-movement.md`, `enemy-spawner.md`, `ui-health-bar.md`
2. Present to developer with rationale
3. Developer approves/modifies/redefines
4. Create task flow files with frontmatter + tags
5. Create issue dependency diagram

**Suggested:** Offer to add tests.md as optional issue

### Phase 5: FINALIZE
Lock design and hand off.

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
