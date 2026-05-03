# Rules: Godot Game

## Architecture Rules

### Rule G1: Node Composition

Use composition over inheritance for nodes.

```gdscript
# ✓ Good: Composition
extends CharacterBody2D

@onready var health = $HealthComponent
@onready var movement = $MovementComponent

func _ready():
    health.died.connect(_on_died)

func _on_died():
    queue_free()
```

### Rule G2: Signals for Communication

Use signals for loose coupling between nodes.

```gdscript
# ✓ Good: Signal
signal health_changed(new_health)

func take_damage(amount):
    health -= amount
    health_changed.emit(health)
```

### Rule G3: Scene Instancing

Use `preload` for known scenes, `load` for dynamic.

```gdscript
# ✓ Good: Preload for known scenes
const ENEMY_SCENE = preload("res://scenes/enemy.tscn")

func spawn_enemy():
    var enemy = ENEMY_SCENE.instantiate()
    add_child(enemy)
```

## Code Style

### Rule G4: GDScript Conventions

- **snake_case** for variables, functions, signals
- **PascalCase** for classes, nodes, constants
- **UPPER_SNAKE_CASE** for constants

```gdscript
const MAX_HEALTH = 100
var current_health: int
signal health_changed

func take_damage(amount: int) -> void:
    current_health -= amount
```

## Performance

### Rule G5: Avoid _process when possible

Use `_process()` only when necessary. Prefer signals or event-driven.

```gdscript
# ✗ Bad: Polling in _process
func _process(delta):
    if Input.is_action_just_pressed("jump"):
        jump()

# ✓ Good: Use _input or signals
func _input(event):
    if event.is_action_pressed("jump"):
        jump()
```

### Rule G6: Use Object Pooling

For frequently spawned nodes (bullets, particles).

## Testing

### Rule G7: Test Business Logic

Test scripts independently of scenes.

```gdscript
# test_health.gd
extends GutTest

func test_take_damage():
    var health = HealthComponent.new()
    health.max_health = 100
    health.current_health = 100
    
    health.take_damage(30)
    
    assert_eq(health.current_health, 70)
```

## Validation Checklist

- [ ] Node composition used
- [ ] Signals for communication
- [ ] Proper lifecycle (_ready, _exit_tree)
- [ ] Type hints used
- [ ] No polling in _process unless needed
- [ ] Tests for business logic
