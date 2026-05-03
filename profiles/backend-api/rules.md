# Rules: Backend API

## Architecture Rules

### Rule B1: Layered Architecture

Separate concerns into layers:
- **Controller/Handler:** HTTP request/response
- **Service:** Business logic
- **Repository:** Data access
- **Model:** Data structures

```python
# ✓ Good: Layered
# controller.py
@app.get("/users/{user_id}")
async def get_user(user_id: int, service: UserService = Depends()):
    return service.get_user(user_id)

# service.py
class UserService:
    def __init__(self, repo: UserRepository):
        self.repo = repo
    
    def get_user(self, user_id: int) -> User:
        return self.repo.find_by_id(user_id)

# repository.py
class UserRepository:
    def find_by_id(self, user_id: int) -> User:
        return db.query(User).filter(User.id == user_id).first()
```

### Rule B2: Input Validation

Validate all inputs at the boundary.

```python
# ✓ Good: Pydantic validation
class CreateUserRequest(BaseModel):
    email: EmailStr
    password: constr(min_length=8)
    name: constr(min_length=1, max_length=100)
```

## Code Style

### Rule B3: Type Hints

Use type hints everywhere (if language supports).

```python
# ✓ Good
def calculate_total(items: list[Item]) -> Decimal:
    return sum(item.price for item in items)
```

### Rule B4: Descriptive Names

```python
# ✓ Good
def get_active_users_by_date_range(start: datetime, end: datetime) -> list[User]:
    ...

# ✗ Bad
def get_users(s, e):
    ...
```

## Error Handling

### Rule B5: Proper HTTP Status Codes

- `200` OK
- `201` Created
- `400` Bad Request (validation errors)
- `401` Unauthorized
- `403` Forbidden
- `404` Not Found
- `409` Conflict
- `422` Unprocessable Entity
- `500` Internal Server Error

### Rule B6: Consistent Error Format

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid input",
    "details": [
      {"field": "email", "message": "Invalid email format"}
    ]
  }
}
```

## Security

### Rule B7: Never Return Stack Traces

```python
# ✗ Bad
try:
    risky_operation()
except Exception as e:
    return {"error": str(e)}  # Leaks internals

# ✓ Good
try:
    risky_operation()
except Exception:
    logger.exception("Operation failed")
    return {"error": "Internal server error"}, 500
```

### Rule B8: Sanitize All Inputs

Prevent injection attacks:
- SQL injection (use ORM/parameterized queries)
- NoSQL injection
- Command injection
- XSS (escape output)

## Testing

### Rule B9: Test Service Layer

```python
def test_user_service_get_user():
    repo = MockUserRepository()
    repo.find_by_id.return_value = User(id=1, name="Test")
    
    service = UserService(repo)
    user = service.get_user(1)
    
    assert user.name == "Test"
```

### Rule B10: Test API Endpoints

```python
def test_get_user(client):
    response = client.get("/users/1")
    assert response.status_code == 200
    assert response.json()["id"] == 1
```

## Validation Checklist

- [ ] Layered architecture maintained
- [ ] Input validation on all endpoints
- [ ] Type hints used
- [ ] Proper HTTP status codes
- [ ] Consistent error format
- [ ] No stack traces in responses
- [ ] SQL injection prevention
- [ ] Service layer tested
- [ ] API endpoints tested
- [ ] Database migrations documented
