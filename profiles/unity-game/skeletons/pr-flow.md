# Flow: PR

> **Profile:** Unity Game  
> **Purpose:** PR lifecycle with Unity-specific validation

---

## Modes

| Mode | How to invoke | When |
|------|---------------|------|
| **pre-pr** | Run the PR flow for `{name}` (default mode) | Before creating PR |
| **feedback** | Run the PR flow for `{name}` in `feedback` mode | PR has comments |
| **capture** | Run the PR flow for `{name}` in `capture` mode | After merge |

---

## Mode: pre-pr

### Step 1: Issue Completeness

Check all task flows have `status: complete` and `accepted-date`.

### Step 2: Documentation Check

- [ ] task-design.md signed off
- [ ] task-technical-design.md complete
- [ ] task-edge-cases.md exists
- [ ] No placeholders

### Step 3: Unity-Specific Validation

**Architecture:**
- [ ] No UnityEngine in pure C# layers
- [ ] Views use `[SerializeField]`, not public fields
- [ ] Lifecycle methods implemented
- [ ] Events unsubscribed in OnDisable/OnDestroy

**Performance:**
- [ ] No GameObject.Find in Update/FixedUpdate
- [ ] Object pooling for frequent instantiation
- [ ] No memory leaks (event subscriptions)

**Assets:**
- [ ] No hardcoded asset paths
- [ ] Prefabs use text serialization
- [ ] No missing script references

**Testing:**
- [ ] EditMode tests for business logic
- [ ] PlayMode tests for integration
- [ ] No test failures

### Step 4: Cross-Issue Integration

- [ ] Components reference each other correctly
- [ ] Scene references are valid
- [ ] Prefab variants properly configured

### Step 5: Report

```
Pre-PR Validation: {task-name}

Documentation:     ✓ PASS
Architecture:      ✓ PASS
Performance:       ⚠ WARNING (1 issue)
Assets:            ✓ PASS
Tests:             ✓ PASS (8/8)
Integration:       ✓ PASS

Warnings:
  1. [PERF] FeatureController uses GetComponent in Update()
     Suggestion: Cache reference in Awake()

Status: READY FOR PR
```

---

## Mode: feedback

Same as generic profile, but with Unity-specific categories:

**Categories:**
- architecture
- style
- logic
- performance
- unity-specific (scene refs, prefabs, serialization)
- naming
- docs

---

## Mode: capture

Same as generic profile.

**Additional Unity captures:**
- Performance patterns discovered
- Component composition patterns
- Asset management lessons

---

## Customization

Add Unity-specific checks:
- Platform-specific validation (mobile, console, VR)
- Asset bundle checks
- Addressables integration
- Multiplayer synchronization
