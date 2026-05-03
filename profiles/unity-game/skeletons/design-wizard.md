# Wizard: Design

> **Profile:** Unity Game  
> **Purpose:** Design Unity features with component architecture and visual diagrams

---

## When to Use

- New game mechanics (blockers, boosters, enemies, abilities)
- UI features (screens, HUD elements, menus)
- System changes (save/load, audio, networking)
- Level design tools

## Input

- Feature idea (from developer)
- Unity project context

## Output

```
docs/features/{feature-name}/
├── DESIGN.md
├── TDD.md
├── EDGE-CASES.md
├── diagrams/
│   ├── 01-class-diagram.puml
│   ├── 02-component-diagram.puml
│   ├── 03-sequence-{flow}.puml
│   └── 04-issue-dependencies.puml
└── issues/
    ├── 01-{descriptive-name}.md    # Dynamically named (e.g., player-health-system)
    ├── 02-{descriptive-name}.md
    └── ...                         # AI proposes based on feature analysis
```

## Prerequisites

- Unity project context loaded
- Developer can describe the feature

---

## Phases

### Phase 1: UNDERSTAND

**Purpose:** Load context and understand the feature.

**Steps:**
1. Read `docs/project/ARCHITECTURE.md`
2. Read `docs/project/CONVENTIONS.md`
3. Read `docs/project/PATTERNS.md`
4. Explore codebase:
   - Search for similar features
   - Read reference implementations
   - Understand component structure
5. Check existing features in `docs/features/`

**Gate:** Confirm understanding — [A]ccept / [F]eedback / [R]eject

---

### Phase 2: DESIGN (Visual-First)

**Purpose:** Create component architecture using diagrams.

**Steps:**
1. Ask developer: "Describe this feature in 1-2 sentences"
2. Generate class diagram showing:
   - MonoBehaviour components
   - ScriptableObject data
   - Pure C# models/services
   - Relationships (inheritance, composition)
3. Present to developer, iterate
4. Generate component diagram showing:
   - GameObjects in scene hierarchy
   - Component composition
   - Prefab structure
5. Present, iterate until approved

**Artifacts:**
- `diagrams/01-class-diagram.puml`
- `diagrams/02-component-diagram.puml`

**Gate:** Diagram approval — [A]ccept / [F]eedback / [R]eject

---

### Phase 3: SPECIFY

**Purpose:** Generate detailed specs from diagrams.

**Steps:**
1. Create `DESIGN.md`:
   - Feature overview
   - User stories
   - Component architecture (with diagram references)
   - Data flow
   - Unity integration (scenes, prefabs, ScriptableObjects)
   - Input handling
   - Visual/audio specifications
   - Edge cases (high-level)
2. Create `TDD.md`:
   - Script breakdown (MonoBehaviour, ScriptableObject, pure C#)
   - Component responsibilities
   - Lifecycle (Awake, Start, OnEnable, OnDestroy)
   - Data persistence
   - Testing strategy (EditMode/PlayMode)
3. Create `EDGE-CASES.md`:
   - Scene loading/unloading
   - Object pooling
   - Null reference risks
   - Performance concerns
4. Generate sequence diagrams for complex flows

**Artifacts:**
- DESIGN.md
- TDD.md
- EDGE-CASES.md
- Sequence diagrams (if needed)

**Gate:** Specification review — [A]ccept / [F]eedback / [R]eject

---

### Phase 4: PLAN

**Purpose:** Create implementation issues based on feature analysis.

**Steps:**
1. **AI analyzes feature** and proposes issue breakdown:
   - Names describe the WORK (kebab-case), not architectural layers
   - Examples:
     - `player-health-system.md`
     - `enemy-ai-behavior.md`
     - `inventory-ui-panel.md`
     - `save-load-system.md`
     - `vfx-particle-controller.md`
   
2. **Present proposal** to developer with rationale:
   ```
   Proposed issues for "Shield Power-Up":
   
   1. shield-data-model.md          # ScriptableObject + stats
   2. shield-controller.md          # Activation/deactivation logic
   3. shield-visual-effect.md       # Particle system + animations
   4. shield-integration.md         # Scene setup, prefab configuration
   
   Suggested: Add tests.md? [Y]/[N]
   
   [A]ccept / [M]odify / [R]edefine
   ```

3. **Developer adjusts:**
   - Add: "Also need shield-sound-effect.md"
   - Remove: "Skip integration for now"
   - Rename: "call it shield-pickup-behavior.md"

4. **Create issue files** with frontmatter:
   ```yaml
   ---
   issue: shield-data-model
   feature: "shield-power-up"
   status: pending
   depends-on: []
   accepted-date: null
   tags: ["unity", "scriptable-object", "data"]
   ---
   ```

5. **Define acceptance criteria** per issue

6. **Create issue dependency diagram**

7. **Present complete plan**

**Artifacts:**
- Dynamically named issue files
- `diagrams/04-issue-dependencies.puml`

**Gate:** Plan review — [A]ccept / [F]eedback / [R]eject

---

### Phase 5: FINALIZE

**Purpose:** Lock design and hand off.

**Steps:**
1. Lock DESIGN.md (immutable after sign-off)
2. Present complete package
3. Explain handoff to implement-wizard

**Gate:** Final approval — [A]ccept / [F]eedback / [R]eject

---

## Unity-Specific Design Patterns

### MonoBehaviour Components

```csharp
// Component with lifecycle
public class PlayerController : MonoBehaviour
{
    [SerializeField] private float _moveSpeed = 5f;
    [SerializeField] private PlayerModel _model;
    
    private InputProvider _input;
    
    private void Awake()
    {
        _input = new UnityInputProvider();
    }
    
    private void OnEnable()
    {
        _model.OnHealthChanged += HandleHealthChanged;
    }
    
    private void OnDisable()
    {
        _model.OnHealthChanged -= HandleHealthChanged;
    }
    
    private void Update()
    {
        var movement = _input.GetMovement();
        transform.position += movement * _moveSpeed * Time.deltaTime;
    }
    
    private void HandleHealthChanged(int newHealth)
    {
        // Update UI or visual feedback
    }
}
```

### ScriptableObject Data

```csharp
// Data asset
[CreateAssetMenu(fileName = "EnemyData", menuName = "Game/Enemy Data")]
public class EnemyData : ScriptableObject
{
    public string EnemyName;
    public int BaseHealth;
    public float MoveSpeed;
    public GameObject Prefab;
}
```

### Pure C# Model

```csharp
// No Unity dependencies
public class PlayerModel
{
    public event Action<int> OnHealthChanged;
    
    public int Health { get; private set; }
    public bool IsAlive => Health > 0;
    
    public void TakeDamage(int damage)
    {
        Health = Math.Max(0, Health - damage);
        OnHealthChanged?.Invoke(Health);
    }
}
```

---

## Customization

Users can customize:
- Add Unity-specific phases (e.g., "Prefab Setup")
- Change component templates
- Add platform-specific considerations (mobile, console, VR)
