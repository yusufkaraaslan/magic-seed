# Wizard: Design

> **Profile:** Backend API

## When to Use
New endpoints, services, or system changes.

## Output
```
docs/features/{name}/
├── DESIGN.md
├── TDD.md
├── EDGE-CASES.md
├── diagrams/
└── issues/
    ├── 01-{descriptive-name}.md    # Dynamically named (e.g., user-repository)
    ├── 02-{descriptive-name}.md
    └── ...                         # AI proposes based on feature analysis
```

## Phases

### Phase 1: UNDERSTAND
Load context, explore codebase.

### Phase 2: DESIGN (Visual-First)
Generate API flow and data model diagrams.

### Phase 3: SPECIFY
Create docs with backend specifics:
- API specification (OpenAPI/GraphQL schema)
- Data models
- Service interfaces
- Authentication/authorization
- Error handling
- Database schema changes

### Phase 4: PLAN
**Purpose:** Create implementation issues based on feature analysis.

**Process:**
1. AI analyzes feature and proposes issues (kebab-case names):
   - Examples: `user-data-model.md`, `auth-service.md`, `api-endpoint.md`, `db-migration.md`
2. Present to developer with rationale
3. Developer approves/modifies/redefines
4. Create issue files with frontmatter + tags
5. Create issue dependency diagram

**Suggested:** Offer to add tests.md as optional issue

### Phase 5: FINALIZE
Lock design.

## Backend Patterns

```python
# FastAPI endpoint
@app.post("/users", response_model=UserResponse)
async def create_user(
    request: CreateUserRequest,
    service: UserService = Depends()
):
    user = service.create_user(request)
    return UserResponse.from_model(user)

# Service layer
class UserService:
    def __init__(self, repo: UserRepository):
        self.repo = repo
    
    def create_user(self, request: CreateUserRequest) -> User:
        user = User(**request.dict())
        return self.repo.save(user)
```
