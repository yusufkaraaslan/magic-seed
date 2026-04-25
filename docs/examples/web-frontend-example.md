# Example: Web Frontend Workflow

Walkthrough of using magic-seed with a React project.

---

## Project Setup

**Project:** `TaskMaster` — React task management app

**Structure:**
```
TaskMaster/
├── src/
│   ├── components/
│   ├── pages/
│   ├── hooks/
│   ├── services/
│   └── store/
├── package.json
└── vite.config.ts
```

---

## Step 1: Initialize

```
Developer: "Initialize magic-seed for this project"

AI: Detecting project type...
    Found: package.json, src/, .tsx files
    Framework: React
    Confidence: 100%

AI: Generating wizards...
    ✓ All 6 wizards created
```

---

## Step 2: Design Feature

**Feature:** Task Categories

```
Developer: "Design a feature called task-categories"

AI: Phase 2: DESIGN (Visual-First)
    
    Class Diagram v1:
    [TaskCategory] ---uses---> [Task]
    [CategoryList] ---uses---> [TaskCategory]
    [useCategories] ---manages---> [CategoryState]
    
    Developer: Add color property to TaskCategory
    
    AI: Updated diagram...
    
    Developer: Accept

AI: Creating docs...
    ✓ DESIGN.md
    ✓ TDD.md
    ✓ EDGE-CASES.md
    
AI: Creating issues...
    1. model.md — Category types and state
    2. components.md — CategoryList, CategoryBadge
    3. hooks.md — useCategories hook
    4. tests.md — Component and hook tests
    5. integration.md — Page integration
```

---

## Step 3: Implement

```
Developer: "Implement the next issue for task-categories"

AI: Issue: model.md
    Creating types...
    ```typescript
    interface TaskCategory {
      id: string;
      name: string;
      color: string;
      taskIds: string[];
    }
    ```
    
    Creating state management...
    ```typescript
    const useCategoryStore = create<CategoryStore>((set) => ({
      categories: [],
      addCategory: (category) =>
        set((state) => ({
          categories: [...state.categories, category]
        })),
    }));
    ```
    
    Developer: Accept
    
[Repeat for components, hooks, tests...]
```

---

## Step 4: PR

```
Developer: "Validate task-categories for PR"

AI: pre-pr validation...
    ✓ Documentation
    ✓ TypeScript types
    ✓ Accessibility labels
    ✓ Tests passing (15/15)
    ✓ Build succeeds
    
    Status: READY
```

---

## Result

Clean feature with:
- Type-safe implementation
- Component tests
- Accessibility labels
- Living documentation

---

## Lessons

1. **Component diagrams helped plan reusability.**
2. **Hook extraction pattern captured for future.**
3. **Accessibility checks caught missing labels.**
