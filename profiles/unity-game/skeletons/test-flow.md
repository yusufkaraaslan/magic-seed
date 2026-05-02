# Flow: Test

> **Profile:** Unity Game  
> **Purpose:** Unity Test Framework integration (EditMode + PlayMode)

---

## When to Use

- After implement-flow completes
- Standalone for existing code
- Before PR

## Input

- Task docs
- Implementation code
- Unity Test Framework setup

## Output

- EditMode tests (pure C#)
- PlayMode tests (Unity integration)
- Coverage report

---

## Phases

### Phase 1: READ

Read task docs, implementation, and edge cases.

### Phase 2: PLAN

Plan test structure:
- EditMode: Models, services, utilities
- PlayMode: MonoBehaviour integration, scene loading

### Phase 3: GENERATE

Create test files:
```
Assets/Tests/
├── EditMode/
│   └── FeatureTests.cs
└── PlayMode/
    └── FeatureIntegrationTests.cs
```

### Phase 4: IMPLEMENT

Write tests:

**EditMode Example:**
```csharp
[Test]
public void TakeDamage_ReducesHealth()
{
    var model = new PlayerModel { Health = 100 };
    model.TakeDamage(30);
    Assert.AreEqual(70, model.Health);
}

[Test]
public void TakeDamage_DoesNotGoBelowZero()
{
    var model = new PlayerModel { Health = 10 };
    model.TakeDamage(30);
    Assert.AreEqual(0, model.Health);
}
```

**PlayMode Example:**
```csharp
[UnityTest]
public IEnumerator HealthComponent_UpdatesUI()
{
    var health = new GameObject().AddComponent<HealthComponent>();
    var ui = new GameObject().AddComponent<HealthUI>();
    
    health.OnHealthChanged += ui.UpdateHealth;
    health.TakeDamage(20);
    
    yield return null; // Wait for frame
    
    Assert.AreEqual("80", ui.HealthText.text);
    
    Object.Destroy(health.gameObject);
    Object.Destroy(ui.gameObject);
}
```

### Phase 5: VALIDATE

Run tests:
```bash
# Via Unity Test Runner
# Or command line:
/Applications/Unity/Unity.app/Contents/MacOS/Unity \
  -runTests \
  -testPlatform EditMode \
  -testResults results.xml
```

### Phase 6: DOCUMENT

Update task-technical-design.md with test info.

---

## Customization

Add:
- Performance tests
- Stress tests
- Platform-specific tests
