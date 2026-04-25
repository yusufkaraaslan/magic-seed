# Slot Catalog Template

> **Applies to:** Profile authors  
> **Purpose:** Template for creating discovery guides that map project-specific values

---

## What is a Slot?

A **slot** is a project-specific value that wizards need to know. Examples:
- Project name
- Source code directory
- Base model class
- Naming conventions
- Framework version

Slots are discovered during magic-seed initialization and filled into wizard skeletons.

---

## Slot Definition

Each slot has these fields:

```markdown
### Slot: {id}

| Field | Value |
|-------|-------|
| **ID** | {unique-identifier} |
| **Description** | What this slot represents |
| **Required** | Yes / No |
| **Discovery** | How to find this value |
| **Fallback** | What to do if not found |
```

---

## Example Slot Catalog

### Category 1: Project Identity

#### Slot: project-name

| Field | Value |
|-------|-------|
| **ID** | project-name |
| **Description** | Human-readable project name |
| **Required** | Yes |
| **Discovery** | Read first heading in `README.md` or `package.json` name field |
| **Fallback** | Ask developer: "What is this project called?" |

#### Slot: source-directory

| Field | Value |
|-------|-------|
| **ID** | source-directory |
| **Description** | Root directory containing source code |
| **Required** | Yes |
| **Discovery** | Check common locations: `src/`, `lib/`, `app/`, `Assets/Scripts/`, `code/` |
| **Fallback** | Ask developer: "Where is your source code?" |

### Category 2: Architecture

#### Slot: architecture-pattern

| Field | Value |
|-------|-------|
| **ID** | architecture-pattern |
| **Description** | Project architecture (MVC, MVVM, Clean, Hexagonal, etc.) |
| **Required** | No |
| **Discovery** | Search `README.md`, `ARCHITECTURE.md`, `docs/` for architecture keywords |
| **Fallback** | Ask developer: "What architecture pattern does this project use?" |

#### Slot: base-model-class

| Field | Value |
|-------|-------|
| **ID** | base-model-class |
| **Description** | Base class for domain models |
| **Required** | No |
| **Discovery** | Search for abstract classes in model/domain layer. Look for classes with `Base`, `Abstract` in name |
| **Fallback** | Assume no base class, or ask developer |

### Category 3: Technology Stack

#### Slot: primary-language

| Field | Value |
|-------|-------|
| **ID** | primary-language |
| **Description** | Main programming language |
| **Required** | Yes |
| **Discovery** | Check file extensions in source directory, `package.json`, `go.mod`, `Cargo.toml`, `pyproject.toml` |
| **Fallback** | Ask developer |

#### Slot: framework

| Field | Value |
|-------|-------|
| **ID** | framework |
| **Description** | Primary framework (React, Unity, Django, etc.) |
| **Required** | No |
| **Discovery** | Check dependencies in package manifest, look for framework-specific files |
| **Fallback** | Ask developer, or record "none" |

### Category 4: Conventions

#### Slot: naming-convention

| Field | Value |
|-------|-------|
| **ID** | naming-convention |
| **Description** | Class/file naming (PascalCase, snake_case, kebab-case) |
| **Required** | No |
| **Discovery** | Scan source files, check for naming patterns |
| **Fallback** | Use profile default, or ask developer |

#### Slot: test-directory

| Field | Value |
|-------|-------|
| **ID** | test-directory |
| **Description** | Where tests live |
| **Required** | No |
| **Discovery** | Look for `test/`, `tests/`, `__tests__/`, `spec/` directories |
| **Fallback** | Assume `tests/` or ask developer |

---

## Discovery Instructions

Discovery is described as natural language instructions for the AI:

```markdown
## Discovery Instructions

### Step 1: Project Identity

Read `README.md` first line to get project name.

If `README.md` doesn't exist:
- Check `package.json` for `name` field
- Check directory name
- Ask developer

### Step 2: Source Directory

Check these locations in order:
1. `src/` — Most common
2. `lib/` — Libraries
3. `app/` — Applications
4. `Assets/Scripts/` — Unity
5. `code/` — Generic

If multiple found, pick the one with the most files.
If none found, ask developer.

### Step 3: Architecture Pattern

Search these files for architecture keywords:
- `README.md`
- `ARCHITECTURE.md`
- `docs/architecture.md`
- `docs/project/ARCHITECTURE.md`

Look for: "MVC", "MVVM", "Clean Architecture", "Hexagonal", "Layered", "Microservices"

If not found, look at directory structure:
- `controllers/`, `models/`, `views/` → likely MVC
- `domain/`, `application/`, `infrastructure/` → likely Clean/Hexagonal
- `components/`, `pages/`, `hooks/` → likely Component-based

If ambiguous, ask developer.
```

---

## Slot Usage in Skeletons

Reference slots using `{{slot-name}}` notation:

```markdown
# Wizard: Design

## Step 1: Create Feature Directory

Create `docs/features/{{feature-name}}/` for the new feature.

## Step 2: Generate Class Diagram

Use {{architecture-pattern}} conventions for the class diagram.
Base model class: {{base-model-class}}
```

During generation, the AI replaces `{{slot-name}}` with discovered values.

---

## Validation

Before generation, verify all required slots are filled:

```markdown
## Validation Checklist

### Required Slots (must be filled)
- [ ] project-name
- [ ] source-directory
- [ ] primary-language

### Optional Slots (may be empty)
- [ ] architecture-pattern
- [ ] base-model-class
- [ ] framework
- [ ] naming-convention
- [ ] test-directory

**If required slots are missing:**
Stop and ask developer. Do not generate wizards with empty required slots.
```

---

## Custom Slots

Profiles can define custom slots beyond the template:

```markdown
### Custom Slot: di-framework

| Field | Value |
|-------|-------|
| **ID** | di-framework |
| **Description** | Dependency injection framework |
| **Required** | No |
| **Discovery** | Search for DI container imports (Zenject, Dagger, Inversify, etc.) |
| **Fallback** | Record "none" or ask developer |
```

Custom slots are profile-specific and only used by that profile's wizards.

---

## Example A: Web API Profile

```markdown
# Slot Catalog: Web API

## Category 1: Project Identity

### Slot: project-name
| **ID** | project-name |
| **Required** | Yes |
| **Discovery** | Read first line of `README.md` or `package.json` name field |
| **Fallback** | Ask developer |

### Slot: api-framework
| **ID** | api-framework |
| **Required** | Yes |
| **Discovery** | Check dependencies: `fastapi`, `django`, `express`, `gin` |
| **Fallback** | Ask developer |

### Slot: database
| **ID** | database |
| **Required** | No |
| **Discovery** | Check dependencies: `psycopg2`, `pymongo`, `redis` |
| **Fallback** | Ask developer |

## Category 2: Architecture

### Slot: auth-method
| **ID** | auth-method |
| **Required** | No |
| **Discovery** | Search for `jwt`, `oauth`, `session` in code |
| **Fallback** | Record "none" |

## Validation

- [ ] project-name: ________________
- [ ] api-framework: _______________
- [ ] database: ____________________
- [ ] auth-method: _________________
```

## Example B: Mobile App Profile

```markdown
# Slot Catalog: Mobile App

## Category 1: Project Identity

### Slot: project-name
| **ID** | project-name |
| **Required** | Yes |
| **Discovery** | Read `pubspec.yaml` (Flutter) or `package.json` (React Native) |
| **Fallback** | Ask developer |

### Slot: platform
| **ID** | platform |
| **Required** | Yes |
| **Discovery** | Check for `ios/`, `android/` directories |
| **Fallback** | Ask developer |

### Slot: state-management
| **ID** | state-management |
| **Required** | No |
| **Discovery** | Check dependencies: `provider`, `riverpod`, `redux`, `zustand` |
| **Fallback** | Record "none" |

## Category 2: UI Framework

### Slot: navigation-library
| **ID** | navigation-library |
| **Required** | No |
| **Discovery** | Check dependencies: `go_router`, `react-navigation` |
| **Fallback** | Record "default" |

## Validation

- [ ] project-name: ________________
- [ ] platform: ____________________
- [ ] state-management: ____________
- [ ] navigation-library: __________
```

---

## Tips for Profile Authors

1. **Start with required slots.** What does every wizard need to know?
2. **Discovery first, ask second.** Only ask what can't be discovered.
3. **Provide fallback questions.** If discovery fails, what should the AI ask?
4. **Validate before generating.** Don't produce wizards with empty required slots.
5. **Document custom slots.** Explain why this profile needs slots beyond the template.
6. **Test discovery.** Run the discovery instructions on a real project and verify slots are filled correctly.
