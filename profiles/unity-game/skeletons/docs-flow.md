# Flow: Docs

> **Profile:** Unity Game
> **Purpose:** Unity-specific documentation maintenance

---

## When to Use

- Documentation outdated
- New team member onboarding
- After major changes

## Phases

### Phase 1: AUDIT

Check Unity-specific docs:
- Component documentation
- Scene setup guides
- Prefab documentation
- ScriptableObject catalogs
- Input system docs

### Phase 2: UPDATE

Update:
- Component diagrams
- Scene hierarchy docs
- Prefab variant explanations
- Inspector configuration guides

### Phase 3: CREATE

Add missing docs:
- New component guides
- Setup tutorials
- Troubleshooting guides

### Phase 4: ORGANIZE

Structure:
```
docs/
├── project/
│   ├── ARCHITECTURE.md
│   ├── CONVENTIONS.md
│   ├── PATTERNS.md
│   └── DECISIONS.md
├── unity/
│   ├── scene-setup.md
│   ├── prefab-guide.md
│   ├── scriptableobjects.md
│   └── input-system.md
├── tasks/
│   └── {task-name}/
└── team/
```

### Phase 5: VERIFY

Check:
- [ ] All components documented
- [ ] Scene setup reproducible
- [ ] Prefab variants explained
- [ ] Code examples work

---

## Unity-Specific Documentation

### Component Documentation Template

```markdown
# Component: {Name}

## Purpose
What this component does.

## Inspector Settings
| Field | Type | Description |
|-------|------|-------------|
| Speed | float | Movement speed |
| Model | PlayerModel | Data model reference |

## Events
| Event | Description |
|-------|-------------|
| OnHealthChanged | Fired when health changes |

## Dependencies
- Requires: Rigidbody
- Optional: Animator

## Usage
```csharp
var controller = gameObject.AddComponent<PlayerController>();
controller.Init(model);
```
```

---

## Customization

Add:
- Video tutorials
- Interactive guides
- API documentation generation
