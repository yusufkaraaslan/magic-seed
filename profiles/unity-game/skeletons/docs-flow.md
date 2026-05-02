# Flow: Docs

> **Profile:** Unity Game
> **Purpose:** Unity-specific documentation maintenance

---

## When to Use

- Documentation outdated
- New team member onboarding
- After major changes

## Phases

This profile follows the canonical **2-phase, 2-gate** structure from `universal/workflow-structure.md` (and `profiles/generic/skeletons/docs-flow.md`). Unity-specific doc concerns below.

### Phase 1: APPLY *(STANDARD gate)*
- **1.1 SCAN** — Audit Unity-specific docs:
  - Component documentation
  - Scene setup guides
  - Prefab documentation
  - ScriptableObject catalogs
  - Input system docs
- **1.2 PROPOSE** — Draft updates:
  - Component diagrams
  - Scene hierarchy docs
  - Prefab variant explanations
  - Inspector configuration guides
  - New component guides / setup tutorials / troubleshooting (if missing)
- **1.3 APPLY** — Apply diffs in place (Rule 2 — never archive). Reorganize structure as needed:
  ```
  flow-storage/project/
  ├── ARCHITECTURE.md
  ├── CONVENTIONS.md
  ├── PATTERNS.md
  └── DECISIONS.md
  flow-storage/team/
  ├── scene-setup.md
  ├── prefab-guide.md
  ├── scriptableobjects.md
  └── input-system.md
  ```
  Verify completeness:
  - [ ] All components documented
  - [ ] Scene setup reproducible
  - [ ] Prefab variants explained
  - [ ] Code examples work
- → **Gate 1: Doc updates review** — audit + applied diffs together.

### Phase 2: COMMIT *(CRITICAL gate)*
- **2.1 COMMIT** *(executes only after the gate is accepted)* — Stage updated docs; commit `docs: update {scope} — {summary}`. Skip 2.1 if `--no-commit`.
- → **Gate 2: Commit review** — presented BEFORE 2.1 executes git commit.

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
