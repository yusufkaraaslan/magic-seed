# Flow: Test

> **Profile:** Godot Game

## When to Use
After implement-flow or standalone.

## Phases

This profile uses the canonical **2-phase, 2-gate** test-flow from `profiles/generic/skeletons/test-flow.md` (Phase 1: WRITE → Phase 2: VERIFY). Godot-specific test patterns (gdUnit4 / GUT) below.

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
