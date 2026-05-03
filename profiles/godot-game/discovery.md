# Discovery Guide: Godot Game

## Step 1: Project Identity

**Slot: project-name**
- Read `project.godot` → `config/name`
- Fallback: directory name

**Slot: game-module**
- Scan for directories with scripts

## Step 2: Language

**Slot: script-language**
- Check for `.gd` files → GDScript
- Check for `.cs` files → C#
- Both present → Ask developer

## Step 3: Architecture

**Slot: node-patterns**
- Check scene files (`.tscn`) for node structure
- Look for common patterns:
  - `Area2D`, `CharacterBody2D` → 2D game
  - `Area3D`, `CharacterBody3D` → 3D game
  - `Control` → UI

## Step 4: Framework

**Slot: signal-system**
- Search for `signal` declarations in `.gd` files
- Search for `connect`, `emit_signal`, `Signal` in C#

**Slot: scene-structure**
- Check `scenes/` or `scene/` directory
- Look for scene organization (levels, ui, entities)

## Step 5: Testing

**Slot: test-framework**
- Check for GUT (Godot Unit Test)
- Check for gdUnit4 (Godot 4)
- Check for custom test setup

---

## Discovery Summary

```
Discovery Complete: Godot Game Profile

Project: {project-name}
  Language: {script-language}

Architecture:
  Type: {2D/3D/UI}
  Nodes: {node-patterns}
  Scenes: {scene-structure}

Communication:
  Signals: {signal-system}

Testing:
  Framework: {test-framework}
```
