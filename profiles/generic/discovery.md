# Discovery Guide: Generic

> **Purpose:** Instructions for AI to explore any project and discover its structure

---

## Step 1: Project Identity

### Slot: project-name

**Discovery:**
1. Read first line of `README.md` (heading)
2. If no README, check `package.json` for `name` field
3. If no package.json, check directory name
4. If still unclear, ask developer

**Example:**
```
Found: "My Awesome Project" (from README.md)
```

### Slot: project-description

**Discovery:**
1. Read `README.md` description paragraph
2. If no README, leave empty

---

## Step 2: Source Directory

### Slot: source-directory

**Discovery:**
Check these locations in order:

1. `src/` — Most common convention
2. `lib/` — Library projects
3. `app/` — Application projects
4. `code/` — Generic
5. `source/` — Alternative

**Selection criteria:**
- Directory exists
- Contains source files (not just config)
- Most files of project's primary language

**If multiple found:**
Present options to developer:
```
Found multiple source directories:
  1. src/ (contains .py files)
  2. lib/ (contains .js files)

Which is the primary source directory?
```

**If none found:**
Ask developer: "Where is your source code located?"

---

## Step 3: Primary Language

### Slot: primary-language

**Discovery:**
1. Count file extensions in source directory
2. Most frequent extension = primary language
3. Map extension to language:
   - `.py` → Python
   - `.js`, `.ts`, `.jsx`, `.tsx` → JavaScript/TypeScript
   - `.go` → Go
   - `.rs` → Rust
   - `.java` → Java
   - `.cs` → C#
   - `.cpp`, `.cc`, `.hpp` → C++
   - `.rb` → Ruby
   - `.php` → PHP
   - `.swift` → Swift
   - `.kt` → Kotlin
   - `.scala` → Scala
   - `.elm` → Elm
   - `.ex`, `.exs` → Elixir
   - `.clj` → Clojure

**If ambiguous** (multiple languages with similar counts):
Present top 3 to developer:
```
This project appears to use multiple languages:
  1. Python (45% of files)
  2. JavaScript (40% of files)
  3. Shell (15% of files)

Which is the primary language for new tasks?
```

---

## Step 4: Architecture Pattern

### Slot: architecture-pattern

**Discovery:**
1. Search for architecture keywords in documentation:
   - `README.md`
   - `ARCHITECTURE.md`
   - `docs/` markdown files
   
2. Look for patterns in directory structure:
   - `controllers/`, `models/`, `views/` → MVC
   - `domain/`, `application/`, `infrastructure/` → Clean Architecture / Hexagonal
   - `components/`, `pages/`, `hooks/` → Component-based
   - `entities/`, `use-cases/`, `adapters/` → Clean Architecture
   - `cmd/`, `internal/`, `pkg/` → Go standard layout
   - `src/modules/`, `src/shared/` → Modular

3. Search for framework-specific patterns:
   - `django/` → Django MVT
   - `flask/` → Flask (micro, no strong pattern)
   - `next/` → Next.js (pages or app router)
   - `react/` → React (component-based)

**If found:** Record pattern with confidence level

**If ambiguous:** Present options:
```
This project's structure suggests one of these patterns:
  1. Layered Architecture (src/layers/)
  2. Modular Architecture (src/modules/)
  3. No strong pattern

Which best describes your architecture?
```

---

## Step 5: Testing Setup

### Slot: test-directory

**Discovery:**
Check for:
- `test/`
- `tests/`
- `__tests__/`
- `spec/`
- `e2e/`
- `integration/`

**If found:** Record path and test framework (from imports)

**If not found:** Ask developer

### Slot: test-framework

**Discovery:**
Search test files for imports:
- `pytest`, `unittest` → Python testing
- `jest`, `mocha`, `vitest` → JavaScript testing
- `nunit`, `xunit` → C# testing
- `go test` → Go testing
- `rspec` → Ruby testing

---

## Step 6: Documentation Structure

### Slot: docs-directory

**Discovery:**
Check for:
- `docs/`
- `Docs/`
- `documentation/`
- `wiki/`

**If found:** Record path and structure

**If not found:** Suggest creating `docs/`

---

## Step 7: Build/Package System

### Slot: build-system

**Discovery:**
Check for build files:
- `package.json` → npm/yarn/pnpm
- `pyproject.toml`, `setup.py`, `requirements.txt` → Python
- `go.mod` → Go modules
- `Cargo.toml` → Rust
- `pom.xml`, `build.gradle` → Java
- `CMakeLists.txt` → C++
- `Makefile` → Make
- `Dockerfile` → Docker

---

## Step 8: Version Control

### Slot: version-control

**Discovery:**
Check for `.git/` directory

**If found:** Record git usage

**If not found:** Note "No version control detected"

---

## Discovery Summary Template

After discovery, present:

```
Discovery Complete: Generic Profile

Project: {project-name}
  Description: {project-description}

Structure:
  Source: {source-directory}
  Language: {primary-language}
  Architecture: {architecture-pattern}
  Tests: {test-directory} ({test-framework})
  Docs: {docs-directory}
  Build: {build-system}
  VCS: {version-control}

Detected Files:
  Total: {N} files
  Source: {N} {language} files
  Tests: {N} test files
  Config: {N} config files

Uncertainties:
  - {anything that needs clarification}

Questions for Developer:
  1. {question 1}
  2. {question 2}
```

---

## Next Steps

After discovery:
1. Present summary to developer
2. Ask clarifying questions
3. Generate flows using discovered slots
4. Write to `.ai-workflow/flows/`
