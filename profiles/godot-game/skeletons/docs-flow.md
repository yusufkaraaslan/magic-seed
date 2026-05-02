# Flow: Docs

> **Profile:** Godot Game

## When to Use
Documentation outdated or new team member.

## Phases

### Phase 1-5
Same as generic docs-flow with Godot specifics.

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
