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

### Phase 1: UNDERSTAND
Load context, explore codebase.

### Phase 2: DESIGN (Visual-First)
Generate component hierarchy and state flow diagrams.

### Phase 3: SPECIFY
Create docs with frontend specifics:
- Component architecture
- State management
- API integration
- Routing
- Styling approach
- Accessibility

### Phase 4: PLAN
**Purpose:** Create implementation task flows based on task analysis.

**Process:**
1. AI analyzes task and proposes task flows (kebab-case names):
   - Examples: `user-auth-hook.md`, `login-form-component.md`, `api-error-handler.md`
2. Present to developer with rationale
3. Developer approves/modifies/redefines
4. Create task flow files with frontmatter + tags
5. Create issue dependency diagram

**Suggested:** Offer to add tests.md as optional issue

### Phase 5: FINALIZE
Lock design.

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
