# Wizard: Docs

> **Profile:** Backend API

## When to Use
Documentation maintenance.

## API Documentation

### Endpoint Documentation Template

```markdown
# Endpoint: {METHOD} {path}

## Description
What this endpoint does.

## Request
```json
{
  "field": "type",
  "required": true
}
```

## Response
### Success (200)
```json
{
  "id": 1,
  "name": "Example"
}
```

### Error (400)
```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid input"
  }
}
```

## Authentication
Required scopes: `read`, `write`

## Examples
```bash
curl -X POST /api/resource \
  -H "Authorization: Bearer {token}" \
  -d '{"name":"Example"}'
```
```

## Customization
Add OpenAPI generation, Postman collection export.
