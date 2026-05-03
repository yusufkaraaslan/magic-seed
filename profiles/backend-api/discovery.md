# Discovery Guide: Backend API

## Step 1: Language and Framework

**Slot: primary-language**
- `pyproject.toml`, `requirements.txt` → Python
- `package.json` → JavaScript/TypeScript
- `go.mod` → Go
- `Cargo.toml` → Rust
- `pom.xml` → Java
- `Gemfile` → Ruby

**Slot: framework**
- Python: `fastapi`, `django`, `flask` in requirements
- Node.js: `express`, `fastify`, `nest` in dependencies
- Go: Router imports (`gin`, `echo`, `fiber`)
- Rust: `actix-web`, `axum` in Cargo.toml

## Step 2: Database

**Slot: database**
- Check dependencies:
  - `psycopg2`, `asyncpg` → PostgreSQL
  - `pymongo` → MongoDB
  - `sqlalchemy`, `django-orm` → SQL (check dialect)
  - `redis` → Redis
- Check config files for connection strings

## Step 3: API Style

**Slot: api-style**
- Search for:
  - `@app.get`, `@router.post` → REST
  - `typeDefs`, `resolvers` → GraphQL
  - `.proto` files → gRPC
  - `RPC` classes → JSON-RPC

## Step 4: Authentication

**Slot: auth**
- Search for:
  - `jwt`, `passport` → JWT/OAuth
  - `bcrypt` → Password hashing
  - `api_key` → API keys
  - `session` → Session-based

## Step 5: Architecture

**Slot: architecture-pattern**
- Check structure:
  - `routes/`, `controllers/`, `services/`, `models/` → MVC/Layered
  - `handlers/`, `repositories/` → Clean Architecture
  - `api/`, `core/` → Modular

---

## Discovery Summary

```
Discovery Complete: Backend API Profile

Project: {project-name}
  Language: {primary-language}
  Framework: {framework}

Data:
  Database: {database}
  ORM: {orm}

API:
  Style: {api-style}
  Auth: {auth}
  Versioning: {versioning}

Architecture:
  Pattern: {architecture-pattern}
```
