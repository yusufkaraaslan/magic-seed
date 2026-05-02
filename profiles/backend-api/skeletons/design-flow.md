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

This profile follows the canonical **2-phase, 2-gate** structure from `universal/workflow-structure.md` (and `profiles/generic/skeletons/design-flow.md`). Backend-API specifics are in the sub-tasks.

### Phase 1: SPECIFY *(STANDARD gate)*
- **1.1 UNDERSTAND** — Load context, explore codebase (controllers/, services/, models/, migrations/).
- **1.2 DESIGN (Visual-First)** — Generate API flow + data model diagrams (class + package + sequence as needed). Render every `.puml` to `.svg`.
- **1.3 SPECIFY** — Create docs with **backend-api specifics**:
  - API specification (OpenAPI/GraphQL schema)
  - Data models
  - Service interfaces
  - Authentication/authorization
  - Error handling
  - Database schema changes
- → **Gate 1: Design package review** — diagrams + spec docs together.

### Phase 2: COMMIT *(CRITICAL gate)*
- **2.1 PLAN** — Decompose into implementation task flows (kebab-case names):
  - Examples: `user-data-model.md`, `auth-service.md`, `api-endpoint.md`, `db-migration.md`
  - Create files with frontmatter + tags
  - Generate task-flow dependency diagram
  - Suggested: offer to add `tests.md` as optional task flow
- **2.2 FINALIZE** — Sign off task-design.md (immutable per Rule 9).
- **2.3 COMMIT** *(executes only after the gate is accepted)* — Stage `flow-storage/tasks/{task-name}/`; commit `design({task-name}): sign off task-design.md and {N} task flows`. Skip 2.3 if `--no-commit`.
- → **Gate 2: Plan + final approval** — presented BEFORE 2.3 executes git commit.

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
