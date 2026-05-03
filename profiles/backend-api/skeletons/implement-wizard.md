# Wizard: Implement

> **Profile:** Backend API

## When to Use
After design-wizard.

## Phases

### Phase 1: READ
Load context.

### Phase 2: PLAN
Plan implementation:
- Models
- Services
- Endpoints
- Tests

### Phase 3: IMPLEMENT
```python
# Model
class User(BaseModel):
    id: int
    email: EmailStr
    name: str
    created_at: datetime

# Repository
class UserRepository:
    def __init__(self, db: Session):
        self.db = db
    
    def find_by_id(self, user_id: int) -> User | None:
        return self.db.query(User).filter(User.id == user_id).first()

# Service
class UserService:
    def __init__(self, repo: UserRepository):
        self.repo = repo
    
    def get_user(self, user_id: int) -> User:
        user = self.repo.find_by_id(user_id)
        if not user:
            raise UserNotFoundError(user_id)
        return user

# Endpoint
@app.get("/users/{user_id}")
async def get_user(
    user_id: int,
    service: UserService = Depends()
):
    return service.get_user(user_id)
```

### Phase 4: AUTO-VALIDATE
- [ ] Type hints
- [ ] Input validation
- [ ] Error handling
- [ ] HTTP status codes

### Phase 5: DOC-SYNC
Update docs.

### Phase 6: DEVELOPER REVIEW
Present.

### Phase 7: UPDATE ISSUE
Knowledge record.

### Phase 8: COMMIT
Commit.

## Resume Logic
Check issue status.
