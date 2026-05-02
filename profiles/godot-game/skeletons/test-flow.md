# Flow: Test

> **Profile:** Godot Game

## When to Use
After implement-flow or standalone.

## Phases

### Phase 1-6
Same as generic test-flow but with Godot specifics.

## Test Examples

```gdscript
# GUT Test
extends GutTest

func test_take_damage():
    var model = FeatureModel.new()
    model.health = 100
    model.take_damage(30)
    assert_eq(model.health, 70)

func test_death_signal():
    var model = FeatureModel.new()
    model.health = 10
    
    var signal_emitted = false
    model.died.connect(func(): signal_emitted = true)
    
    model.take_damage(20)
    assert_true(signal_emitted)
```

## Customization
Add scene integration tests.
