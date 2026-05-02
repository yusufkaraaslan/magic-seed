# Rules: Unity Game

> **Applies to:** Unity/C# projects  
> **Purpose:** Unity-specific conventions and guardrails

---

## Architecture Rules

### Rule U1: Separate UnityEngine from Business Logic

Pure C# layers (Models, Services, Domain) must not depend on UnityEngine.

**Why:** Enables testing outside Unity, improves portability.

**Check:**
- No `using UnityEngine;` in Model/Domain files
- No `MonoBehaviour` in business logic
- Unity-specific code only in View/Presentation layer

**Example:**
```csharp
// ✓ Good: Pure C# model
public class PlayerModel
{
    public int Health { get; private set; }
    public void TakeDamage(int amount) { ... }
}

// ✗ Bad: Unity dependency in model
public class PlayerModel : MonoBehaviour  // Wrong layer
{
    public int Health;
}
```

### Rule U2: Views Receive Dependencies via Init()

Views (MonoBehaviour) must not use `[Inject]` or constructor injection.

**Why:** Unity manages MonoBehaviour lifecycle, not DI container.

**Pattern:**
```csharp
public class PlayerView : MonoBehaviour
{
    [SerializeField] private Transform _visualRoot;
    
    private PlayerController _controller;
    
    public void Init(PlayerController controller)
    {
        _controller = controller;
    }
}
```

### Rule U3: Respect MonoBehaviour Lifecycle

Implement proper lifecycle methods:

```csharp
public class GameComponent : MonoBehaviour
{
    private void Awake() { /* Initialize internal state */ }
    private void OnEnable() { /* Subscribe to events */ }
    private void Start() { /* Begin operation */ }
    private void OnDisable() { /* Unsubscribe from events */ }
    private void OnDestroy() { /* Cleanup */ }
}
```

**Critical:** Always unsubscribe in `OnDisable()` or `OnDestroy()` to prevent memory leaks.

---

## Code Style Rules

### Rule U4: C# Naming Conventions

- **Classes/Interfaces:** `PascalCase` (`PlayerController`, `IPlayerModel`)
- **Methods:** `PascalCase` (`TakeDamage()`, `Initialize()`)
- **Properties:** `PascalCase` (`Health`, `IsAlive`)
- **Private fields:** `_camelCase` (`_health`, `_controller`)
- **Public fields:** Avoid; use properties instead
- **Constants:** `UPPER_SNAKE_CASE` (`MAX_HEALTH`)
- **Events:** `PascalCase` + `On` prefix (`OnHealthChanged`)

### Rule U5: SerializeField over Public Fields

Use `[SerializeField]` for Unity inspector visibility, not public fields.

```csharp
// ✓ Good
[SerializeField] private float _moveSpeed;

// ✗ Bad
public float MoveSpeed;  // Breaks encapsulation
```

---

## Performance Rules

### Rule U6: Avoid GameObject.Find in Update()

Cache references in `Awake()` or `Start()`.

```csharp
// ✓ Good
private void Awake()
{
    _player = GameObject.FindWithTag("Player").GetComponent<PlayerView>();
}

// ✗ Bad
private void Update()
{
    var player = GameObject.Find("Player");  // Slow!
}
```

### Rule U7: Use Object Pooling for Frequent Instantiation

Instantiate/Destroy causes GC pressure. Use object pools for:
- Projectiles
- Enemies
- Particle effects
- UI elements

### Rule U8: Prefer struct over class for small data

```csharp
// ✓ Good: 16 bytes or less
public struct GridPosition
{
    public int X;
    public int Y;
}

// ✗ Bad: Unnecessary allocation
public class GridPosition { ... }
```

---

## Testing Rules

### Rule U9: EditMode for Pure C#, PlayMode for Integration

- **EditMode tests:** Business logic, models, utilities
- **PlayMode tests:** Scene loading, MonoBehaviour interactions, integration

### Rule U10: Mock Unity Dependencies

Use interfaces to mock Unity-specific dependencies in tests.

```csharp
public interface IInputProvider
{
    Vector2 GetMovement();
    bool IsJumpPressed();
}

// Real implementation
public class UnityInputProvider : IInputProvider { ... }

// Mock for testing
public class MockInputProvider : IInputProvider { ... }
```

---

## Asset Rules

### Rule U11: No Hardcoded Asset Paths

Use `ScriptableObject` references or Addressables, not string paths.

```csharp
// ✓ Good
[SerializeField] private GameObject _enemyPrefab;

// ✗ Bad
var enemy = Resources.Load<GameObject>("Prefabs/Enemy");  // Fragile
```

### Rule U12: Version Control for Scenes and Prefabs

- Use text serialization for scenes and prefabs
- Enable "Force Text" in Project Settings
- Use prefab variants for variations

---

## Async Rules

### Rule U13: Prefer UniTask over Coroutines

UniTask provides better performance and composability.

```csharp
// ✓ Good: UniTask
public async UniTask MoveToPosition(Vector3 target, CancellationToken ct)
{
    while (Vector3.Distance(transform.position, target) > 0.1f)
    {
        transform.position = Vector3.MoveTowards(...);
        await UniTask.Yield(PlayerLoopTiming.PostLateUpdate, ct);
    }
}

// ✗ Bad: Coroutine
public IEnumerator MoveToPosition(Vector3 target) { ... }
```

### Rule U14: Always Check Cancellation

After every await, check if operation was cancelled.

```csharp
public async UniTask DoSomething(CancellationToken ct)
{
    await UniTask.Delay(1000, cancellationToken: ct);
    
    if (ct.IsCancellationRequested)
        return;
    
    // Continue...
}
```

---

## Validation Checklist

Before considering Unity task complete:

- [ ] No UnityEngine in pure C# layers
- [ ] Views use Init() not [Inject]
- [ ] Lifecycle methods implemented (Awake/OnEnable/Start/OnDisable/OnDestroy)
- [ ] Events unsubscribed in OnDisable/OnDestroy
- [ ] Naming conventions followed
- [ ] SerializeField used instead of public fields
- [ ] No GameObject.Find in Update/FixedUpdate
- [ ] Object pooling for frequent instantiation
- [ ] EditMode tests for business logic
- [ ] PlayMode tests for integration
- [ ] No hardcoded asset paths
- [ ] Async operations handle cancellation
- [ ] Scenes/prefabs use text serialization
