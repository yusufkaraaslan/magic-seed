# Flow: Test

> **Profile:** Backend API

## When to Use
After implement-flow.

## Test Examples

```python
# Unit test
def test_user_service_get_user():
    repo = MockUserRepository()
    repo.find_by_id.return_value = User(id=1, name="Test")
    
    service = UserService(repo)
    user = service.get_user(1)
    
    assert user.name == "Test"

# API test
def test_get_user(client):
    response = client.get("/users/1")
    assert response.status_code == 200
    assert response.json()["id"] == 1

# Integration test
def test_create_user_flow(client, db):
    response = client.post("/users", json={
        "email": "test@example.com",
        "name": "Test User"
    })
    assert response.status_code == 201
    
    user = db.query(User).filter(User.email == "test@example.com").first()
    assert user is not None
```

## Customization
Add contract tests, performance tests.
