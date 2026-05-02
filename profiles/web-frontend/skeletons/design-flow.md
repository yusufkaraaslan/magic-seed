# Flow: Design

> **Profile:** Web Frontend

## When to Use
New tasks, components, pages, or system changes.

## Output
```
flow-storage/tasks/{task-name}/
├── design/
│   ├── task-design.md
│   ├── task-technical-design.md
│   ├── task-edge-cases.md
│   └── diagrams/
└── implement/
    └── flow-plan/
        ├── task-flow-01-{descriptive-name}.md   # Dynamically named (e.g., user-auth-hook)
        ├── task-flow-02-{descriptive-name}.md
        └── ...                                  # AI proposes based on task analysis
```

## Phases

This profile follows the canonical **2-phase, 2-gate** structure from `universal/workflow-structure.md` (and `profiles/generic/skeletons/design-flow.md`). Web-frontend specifics are in the sub-tasks.

### Phase 1: SPECIFY *(STANDARD gate)*
- **1.1 UNDERSTAND** — Load context, explore codebase (components/, hooks/, routes/, stores/).
- **1.2 DESIGN (Visual-First)** — Generate component hierarchy + state flow diagrams (class + package + sequence as needed). Render every `.puml` to `.svg`.
- **1.3 SPECIFY** — Create docs with **web-frontend specifics**:
  - Component architecture
  - State management
  - API integration
  - Routing
  - Styling approach
  - Accessibility
- → **Gate 1: Design package review** — diagrams + spec docs together.

### Phase 2: COMMIT *(CRITICAL gate)*
- **2.1 PLAN** — Decompose into implementation task flows (kebab-case names):
  - Examples: `user-auth-hook.md`, `login-form-component.md`, `api-error-handler.md`
  - Create files with frontmatter + tags
  - Generate task-flow dependency diagram
  - Suggested: offer to add `tests.md` as optional task flow
- **2.2 FINALIZE** — Sign off task-design.md (immutable per Rule 9).
- **2.3 COMMIT** *(executes only after the gate is accepted)* — Stage `flow-storage/tasks/{task-name}/`; commit `design({task-name}): sign off task-design.md and {N} task flows`. Skip 2.3 if `--no-commit`.
- → **Gate 2: Plan + final approval** — presented BEFORE 2.3 executes git commit.

## Frontend Patterns

```tsx
// Component with hooks
export const UserProfile: React.FC<UserProfileProps> = ({ userId }) => {
  const { user, loading } = useUser(userId);
  
  if (loading) return <LoadingSpinner />;
  
  return (
    <article className="user-profile">
      <h2>{user.name}</h2>
      <UserDetails user={user} />
    </article>
  );
};
```
