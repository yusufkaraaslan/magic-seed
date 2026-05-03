# Wizard: Implement

> **Profile:** Unity Game  
> **Purpose:** Implement Unity features with proper lifecycle and component architecture

---

## When to Use

After `/design-wizard` completes and design is signed off.

## Input

- Issue files from `docs/features/{name}/issues/`
- DESIGN.md and TDD.md
- Unity project context

## Output

- C# scripts (MonoBehaviour, ScriptableObject, pure C#)
- Unity assets (prefabs, ScriptableObject assets)
- Tests (EditMode/PlayMode)
- Updated issue files (knowledge records)

## Prerequisites

- Design signed off
- Issue files created

---

## Phases

### Phase 1: READ

**Purpose:** Gather context for this issue.

**Steps:**
1. Load project context
2. Read issue file (acceptance criteria, files to create)
3. Read design docs (relevant sections)
4. Read completed prior issues
5. Find reference implementations in codebase

**Gate:** Confirm context — [A]ccept / [M]odify / [R]eject

---

### Phase 2: PLAN

**Purpose:** Plan Unity-specific implementation.

**Steps:**
1. Analyze acceptance criteria
2. Map to Unity patterns:
   - MonoBehaviour lifecycle
   - Component composition
   - ScriptableObject usage
3. Identify assets needed:
   - Scripts to create
   - Prefabs to create/modify
   - ScriptableObject assets
4. Plan tests (EditMode for logic, PlayMode for integration)

**Gate:** Plan review — [A]ccept / [F]eedback / [R]eject

---

### Phase 3: IMPLEMENT

**Purpose:** Write Unity code.

**Steps:**
1. Create scripts following Unity conventions:
   ```csharp
   // Model (pure C#)
   public class FeatureModel { ... }
   
   // Controller (MonoBehaviour)
   public class FeatureController : MonoBehaviour { ... }
   
   // View (MonoBehaviour)
   public class FeatureView : MonoBehaviour { ... }
   
   // Data (ScriptableObject)
   [CreateAssetMenu(...)]
   public class FeatureData : ScriptableObject { ... }
   ```
2. Implement lifecycle:
   - `Awake()` — internal initialization
   - `OnEnable()` — event subscription
   - `Start()` — external initialization
   - `OnDisable()` — event unsubscription
   - `OnDestroy()` — cleanup
3. Handle Unity specifics:
   - `[SerializeField]` for inspector visibility
   - `RequireComponent` where appropriate
   - `Header` and `Tooltip` attributes
4. Follow profile rules:
   - No UnityEngine in pure C# layers
   - Views use Init() not [Inject]
   - Proper event cleanup

**Artifacts:**
- Source files
- Updated issue (in_progress)

**Gate:** Implementation review — [A]ccept / [F]eedback / [R]eject

---

### Phase 4: AUTO-VALIDATE

**Purpose:** Run Unity-specific checks.

**Steps:**
1. Check architecture:
   - [ ] No `using UnityEngine;` in Model/Domain files
   - [ ] Views use `[SerializeField]` not public fields
   - [ ] Lifecycle methods implemented
2. Check Unity patterns:
   - [ ] Events unsubscribed in OnDisable/OnDestroy
   - [ ] No GameObject.Find in Update
   - [ ] Object pooling considered
3. Check code style:
   - [ ] Naming conventions
   - [ ] Comments for public APIs
4. Run tests if available

**Artifacts:**
- Validation report

**Gate:** Validation review — [A]ccept / [F]eedback / [R]eject

---

### Phase 5: DOC-SYNC

**Purpose:** Update living docs.

**Steps:**
1. Update TDD.md with deviations
2. Update EDGE-CASES.md with discoveries
3. Update PATTERNS.md if new pattern found
4. Add implementation notes to issue

**Gate:** Doc update review — [A]ccept / [F]eedback / [R]eject

---

### Phase 6: DEVELOPER REVIEW

**Purpose:** Present for approval.

**Steps:**
1. Show files created/modified
2. Show component hierarchy (if applicable)
3. Show validation results
4. Highlight key decisions

**Gate:** [A]ccept / [F]eedback / [R]eject

---

### Phase 7: UPDATE ISSUE

**Purpose:** Transform to knowledge record.

**Steps:**
1. Update frontmatter:
   ```yaml
   status: complete
   accepted-date: 2026-04-23
   ```
2. Add implementation notes:
   - Scripts created
   - Prefabs created
   - Components used
   - Deviations from design

**Artifacts:**
- Knowledge record

---

### Phase 8: COMMIT

**Purpose:** Commit the work.

**Steps:**
1. Stage scripts and assets
2. Commit:
   ```
   feat({feature-name}): {issue-type}
   
   - Add FeatureModel, FeatureController, FeatureView
   - Add FeatureData ScriptableObject
   - Implement lifecycle and event handling
   
   Refs: #{issue-number}
   ```

---

## Unity Implementation Patterns

### Component with Events

```csharp
public class HealthComponent : MonoBehaviour
{
    [SerializeField] private int _maxHealth = 100;
    
    public event Action<int, int> OnHealthChanged; // current, max
    public event Action OnDied;
    
    public int CurrentHealth { get; private set; }
    
    private void Awake()
    {
        CurrentHealth = _maxHealth;
    }
    
    public void TakeDamage(int damage)
    {
        if (CurrentHealth <= 0) return;
        
        CurrentHealth = Mathf.Max(0, CurrentHealth - damage);
        OnHealthChanged?.Invoke(CurrentHealth, _maxHealth);
        
        if (CurrentHealth == 0)
            OnDied?.Invoke();
    }
    
    public void Heal(int amount)
    {
        if (CurrentHealth <= 0) return;
        
        CurrentHealth = Mathf.Min(_maxHealth, CurrentHealth + amount);
        OnHealthChanged?.Invoke(CurrentHealth, _maxHealth);
    }
}
```

### ScriptableObject Configuration

```csharp
[CreateAssetMenu(fileName = "WeaponConfig", menuName = "Game/Weapon Config")]
public class WeaponConfig : ScriptableObject
{
    public string WeaponName;
    public int Damage;
    public float FireRate;
    public float Range;
    public GameObject ProjectilePrefab;
    public AudioClip FireSound;
}
```

### Object Pooling

```csharp
public class ObjectPool : MonoBehaviour
{
    [SerializeField] private GameObject _prefab;
    [SerializeField] private int _initialSize = 10;
    
    private Queue<GameObject> _pool = new Queue<GameObject>();
    
    private void Awake()
    {
        for (int i = 0; i < _initialSize; i++)
            CreateNewObject();
    }
    
    private void CreateNewObject()
    {
        var obj = Instantiate(_prefab, transform);
        obj.SetActive(false);
        _pool.Enqueue(obj);
    }
    
    public GameObject Get()
    {
        if (_pool.Count == 0)
            CreateNewObject();
        
        var obj = _pool.Dequeue();
        obj.SetActive(true);
        return obj;
    }
    
    public void Return(GameObject obj)
    {
        obj.SetActive(false);
        _pool.Enqueue(obj);
    }
}
```

---

## Resume Logic

Check for existing progress:
1. Read all issue files
2. Find first pending/in_progress
3. Ask: "Resume from {issue}?"

---

## Customization

Users can customize:
- Add Unity-specific validations
- Change component templates
- Add platform-specific code (mobile input, VR, etc.)
