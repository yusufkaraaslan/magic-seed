# Wizard: Design

> **Profile:** Web Frontend

## When to Use
New features, components, pages, or system changes.

## Output
```
docs/features/{name}/
├── DESIGN.md
├── TDD.md
├── EDGE-CASES.md
├── diagrams/
└── issues/
    ├── 01-{descriptive-name}.md    # Dynamically named (e.g., user-auth-hook)
    ├── 02-{descriptive-name}.md
    └── ...                         # AI proposes based on feature analysis
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
**Purpose:** Create implementation issues based on feature analysis.

**Process:**
1. AI analyzes feature and proposes issues (kebab-case names):
   - Examples: `user-auth-hook.md`, `login-form-component.md`, `api-error-handler.md`
2. Present to developer with rationale
3. Developer approves/modifies/redefines
4. Create issue files with frontmatter + tags
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
