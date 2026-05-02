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

This profile follows the canonical **2-phase, 2-gate** structure from `universal/workflow-structure.md` (and `profiles/generic/skeletons/test-flow.md`). Unity-specific testing details below.

### Phase 1: WRITE *(STANDARD gate)*

**Sub-tasks (auto-proceed):**

**1.1 READ** — Read task docs, implementation, and edge cases.

**1.2 PLAN** — Test structure:
- **EditMode**: Models, services, utilities (pure C#, no Unity runtime)
- **PlayMode**: MonoBehaviour integration, scene loading (Unity runtime required)

**1.3 GENERATE** — Create test file structure:
```
Assets/Tests/
├── EditMode/
│   └── FeatureTests.cs
└── PlayMode/
    └── FeatureIntegrationTests.cs
```

**1.4 IMPLEMENT** — Write tests:

EditMode example:
```csharp
[Test]
public void TakeDamage_ReducesHealth()
{
    var model = new PlayerModel { Health = 100 };
    model.TakeDamage(30);
    Assert.AreEqual(70, model.Health);
}
```

PlayMode example:
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

→ **Gate 1: Test code review** — outline + stubs + filled tests together. No tests run yet.

### Phase 2: VERIFY *(STANDARD gate)*

**Sub-tasks (auto-proceed):**

**2.1 VALIDATE** — Run Unity tests (NEVER skip):
```bash
# Via Unity Test Runner, or command line:
/Applications/Unity/Unity.app/Contents/MacOS/Unity \
  -runTests \
  -testPlatform EditMode \
  -testResults results.xml
```
If failures, surface at the gate; do not paper over.

**2.2 DOCUMENT** — Update task-technical-design.md with test info; append summary to `flow-storage/tasks/{task-name}/test/`; update PATTERNS.md if new test patterns emerged.

→ **Gate 2: Results review** — test results, coverage delta, new edge cases.

Note: test-flow does NOT git commit. Tests are committed by implement-flow (alongside the code) or pr-flow (as part of validation).

---

## Customization

Add:
- Performance tests
- Stress tests
- Platform-specific tests
