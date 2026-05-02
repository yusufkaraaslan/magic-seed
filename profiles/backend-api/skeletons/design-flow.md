# Flow: Design

> **Profile:** Backend API

## When to Use
New endpoints, services, or system changes.

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
        ├── task-flow-01-{descriptive-name}.md   # Dynamically named (e.g., user-repository)
        ├── task-flow-02-{descriptive-name}.md
        └── ...                                  # AI proposes based on task analysis
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
**Purpose:** Create implementation task flows based on task analysis.

**Process:**
1. AI analyzes task and proposes task flows (kebab-case names):
   - Examples: `user-data-model.md`, `auth-service.md`, `api-endpoint.md`, `db-migration.md`
2. Present to developer with rationale
3. Developer approves/modifies/redefines
4. Create task flow files with frontmatter + tags
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
