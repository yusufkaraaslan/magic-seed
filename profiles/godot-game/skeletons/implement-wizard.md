# Wizard: Implement

> **Profile:** Godot Game

## When to Use
After design-wizard completes.

## Phases

### Phase 1: READ
Load context, read issue, design docs.

### Phase 2: PLAN
Plan GDScript implementation:
- Script structure
- Node hierarchy
- Signal connections

### Phase 3: IMPLEMENT
Create GDScript files:
```gdscript
extends Node
class_name FeatureController

@export var model: FeatureModel
@onready var view = $FeatureView

func _ready():
    model.updated.connect(_on_model_updated)

func _on_model_updated():
    view.update_display(model.data)
```

### Phase 4: AUTO-VALIDATE
- [ ] Type hints used
- [ ] Signals connected/disconnected
- [ ] Node paths valid
- [ ] _ready() initialization

### Phase 5: DOC-SYNC
Update docs.

### Phase 6: DEVELOPER REVIEW
Present for approval.

### Phase 7: UPDATE ISSUE
Transform to knowledge record.

### Phase 8: COMMIT
Commit the work.

## Resume Logic
Check issue status and resume.
