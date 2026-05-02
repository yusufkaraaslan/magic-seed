# Flow: Docs

> **Profile:** Godot Game

## When to Use
Documentation outdated or new team member.

## Phases

This profile uses the canonical **2-phase, 2-gate** docs-flow from `profiles/generic/skeletons/docs-flow.md` (Phase 1: APPLY → Phase 2: COMMIT). Godot-specific documentation templates below.

## Godot Documentation

### Node Documentation Template

```markdown
# Node: {Name}

## Scene Path
`res://scenes/{name}.tscn`

## Parent Node
{ParentNode}

## Scripts
| Script | Purpose |
|--------|---------|
| {name}.gd | Main logic |

## Signals
| Signal | Description |
|--------|-------------|
| {signal} | {description} |

## Exported Variables
| Variable | Type | Description |
|----------|------|-------------|
| {var} | {type} | {desc} |

## Usage
```gdscript
var node = preload("res://scenes/{name}.tscn").instantiate()
add_child(node)
```
```

## Customization
Add signal flow diagrams.
