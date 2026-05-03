# Wizard: Design

> **Profile:** Godot Game

## When to Use
Starting new features in Godot.

## Output
```
docs/features/{name}/
├── DESIGN.md
├── TDD.md
├── EDGE-CASES.md
├── diagrams/
└── issues/
    ├── 01-{descriptive-name}.md    # Dynamically named (e.g., player-movement)
    ├── 02-{descriptive-name}.md
    └── ...                         # AI proposes based on feature analysis
```

## Phases

### Phase 1: UNDERSTAND
Load project context and explore codebase.

### Phase 2: DESIGN (Visual-First)
Generate node hierarchy diagram and script architecture.

### Phase 3: SPECIFY
Create DESIGN.md, TDD.md, EDGE-CASES.md with Godot specifics:
- Node tree structure
- Script attachments
- Signal connections
- Scene instancing

### Phase 4: PLAN
**Purpose:** Create implementation issues based on feature analysis.

**Process:**
1. AI analyzes feature and proposes issues (kebab-case names):
   - Examples: `player-movement.md`, `enemy-spawner.md`, `ui-health-bar.md`
2. Present to developer with rationale
3. Developer approves/modifies/redefines
4. Create issue files with frontmatter + tags
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
