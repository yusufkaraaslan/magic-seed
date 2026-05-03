# Example: Unity Game Workflow

Walkthrough of using magic-seed with a Unity project.

---

## Project Setup

**Project:** `SpaceDefender` — Unity 2D space shooter

**Structure:**
```
SpaceDefender/
├── Assets/
│   ├── Scripts/
│   │   ├── Controllers/
│   │   ├── Models/
│   │   └── Views/
│   └── Scenes/
├── ProjectSettings/
└── Packages/
```

---

## Step 1: Initialize

```
Developer: /magic-seed init

AI: Detecting project type...
    Found: Assets/, *.cs, ProjectSettings/
    Confidence: 100% — Unity project

AI: Loading Unity Game profile...

AI: Running discovery...
    Project: SpaceDefender
    Module: Assets/Scripts/
    Namespace: SpaceDefender
    DI: None detected
    Async: Coroutines
    Features: Controllers, Models, Views

AI: Generating wizards...
    ✓ design-wizard.md
    ✓ implement-wizard.md
    ✓ pr-wizard.md
    ✓ test-wizard.md
    ✓ deploy-wizard.md
    ✓ docs-wizard.md

AI: Install complete!
    Wizards available in .ai-workflow/wizards/
```

---

## Step 2: Design New Feature

**Feature:** Shield Power-Up

```
Developer: /design-wizard shield-powerup

AI: Phase 1: UNDERSTAND
    Loading SpaceDefender architecture...
    Found similar features: SpeedBoost, DoubleShot
    Reading reference implementations...

AI: Phase 2: DESIGN (Visual-First)
    
    Class Diagram v1:
    [ShieldPowerUp] ---extends---> [PowerUpBase]
    [ShieldPowerUp] ---uses---> [ShieldController]
    [ShieldController] ---uses---> [PlayerModel]
    
    Developer: Add ShieldView component
    
    AI: Class Diagram v2:
    [ShieldPowerUp] ---extends---> [PowerUpBase]
    [ShieldPowerUp] ---uses---> [ShieldController]
    [ShieldController] ---uses---> [PlayerModel]
    [ShieldController] ---uses---> [ShieldView]
    
    Developer: Accept

AI: Phase 3: SPECIFY
    Creating DESIGN.md...
    Creating TDD.md...
    Creating EDGE-CASES.md...
    
    Developer: Accept

AI: Phase 4: PLAN
    Issues:
    1. model.md — Shield data model
    2. controller.md — Shield logic
    3. view.md — Shield visual
    4. tests.md — Unit + PlayMode tests
    5. integration.md — Scene setup, prefab
    
    Developer: Accept

AI: Phase 5: FINALIZE
    Design locked!
    Handing off to implement-wizard...
```

---

## Step 3: Implement

```
Developer: /implement-wizard shield-powerup

AI: Resuming from first pending issue...
    Issue: model.md
    
AI: Phase 1: READ
    Loading context...
    Reading ShieldPowerUp design...
    Reference: SpeedBoost implementation

AI: Phase 2: PLAN
    Files to create:
    - Assets/Scripts/Models/ShieldModel.cs
    - Assets/Scripts/Controllers/ShieldController.cs
    
    Developer: Accept

AI: Phase 3: IMPLEMENT
    Creating ShieldModel.cs...
    Creating ShieldController.cs...
    
    Developer: Accept

AI: Phase 4: AUTO-VALIDATE
    ✓ No UnityEngine in Model
    ✓ Controller has lifecycle methods
    ✓ Events unsubscribed in OnDisable
    
    Developer: Accept

AI: Phase 7: UPDATE ISSUE
    model.md → status: complete
    
AI: Committing...
    feat(shield-powerup): model implementation
    
AI: Next issue: controller.md
    
[Repeat for all issues...]
```

---

## Step 4: PR

```
Developer: /pr-wizard shield-powerup

AI: Mode: pre-pr
    
    Step 1: Issues complete ✓
    Step 2: Documentation ✓
    Step 3: Architecture ✓
    Step 4: Performance ✓
    Step 5: Tests ✓ (12/12 passing)
    Step 6: Integration ✓
    
    Status: READY FOR PR
    
Developer: [Creates PR]

[Reviewers comment...]

Developer: /pr-wizard shield-powerup --mode=feedback

AI: Processing feedback...
    Comment 1: "Shield duration should be configurable"
    → Created feedback-1.md
    → Suggested fix: Add [SerializeField] duration
    Developer: Yes
    
    Comment 2: "Missing null check in controller"
    → Created feedback-2.md
    → Suggested fix: Add null check
    Developer: Yes
    
AI: Committing fixes...
    fix(shield-powerup): address PR feedback
    
[PR merged...]

Developer: /pr-wizard shield-powerup --mode=capture

AI: Capturing knowledge...
    ✓ lessons-learned.md created
    ✓ PATTERNS.md updated (Shield pattern)
    ✓ CONVENTIONS.md updated (null checks)
    
    Feature complete!
```

---

## Result

**Knowledge Base:**
```
docs/features/shield-powerup/
├── DESIGN.md (signed off)
├── TDD.md
├── EDGE-CASES.md
├── diagrams/
│   ├── 01-class-diagram.puml
│   ├── 02-package-diagram.puml
│   └── 04-issue-dependencies.puml
├── issues/
│   ├── 01-model.md (knowledge record)
│   ├── 02-controller.md (knowledge record)
│   ├── 03-view.md (knowledge record)
│   ├── 04-tests.md (knowledge record)
│   └── 05-integration.md (knowledge record)
├── feedback/
│   ├── feedback-1-review.md
│   └── feedback-2-review.md
└── lessons-learned.md
```

**Project Knowledge Updated:**
- `docs/project/PATTERNS.md` — Shield pattern added
- `docs/project/CONVENTIONS.md` — Null check convention added

---

## Lessons

1. **Visual-first design caught missing component early.**
2. **Per-issue commits kept history clean.**
3. **Feedback issues captured review knowledge permanently.**
4. **Patterns catalog grew — next power-up will be faster.**
