# Wizard: Test

> **Profile:** Generic  
> **Purpose:** Plan, generate, and validate tests for features

---

## When to Use

- After the implement wizard completes (for new features)
- Standalone for existing code that lacks tests
- Before PR to ensure coverage

## Input

- Feature docs (DESIGN.md, TDD.md)
- Implementation code
- Edge cases from EDGE-CASES.md

## Output

- Test files
- Coverage report
- Updated issue files

## Prerequisites

- Test framework is identified (from discovery)
- Feature implementation exists (or is planned)

---

## Phases

### Phase 1: READ

**Purpose:** Understand what needs testing.

**Steps:**
1. Read feature docs:
   - DESIGN.md (requirements)
   - TDD.md (technical details)
   - EDGE-CASES.md (boundary conditions)
2. Read implementation code
3. Identify testable units:
   - Functions/methods
   - Classes/modules
   - Integration points
4. Check existing tests:
   - What's already tested?
   - What's missing?

**Artifacts:**
- Test plan (mental model)

**Gate:** Confirm scope — [A]ccept / [M]odify / [R]eject

---

### Phase 2: PLAN

**Purpose:** Structure the test suite.

**Steps:**
1. Define test categories:
   - Unit tests (business logic)
   - Integration tests (component interaction)
   - Edge case tests (boundaries, errors)
   - E2E tests (if applicable)
2. Map tests to requirements:
   - Each acceptance criterion → test case(s)
   - Each edge case → test case
3. Identify mocks/stubs needed
4. Plan test data

**Artifacts:**
- Test plan document (temporary)

**Gate:** Plan review — [A]ccept / [F]eedback / [R]eject

---

### Phase 3: GENERATE

**Purpose:** Create test file structure and stubs.

**Steps:**
1. Create test files following project conventions:
   ```
   {{test-dir}}/
   ├── {{feature}}_test.{{ext}}
   └── ... (number and names vary by project)
   ```
   
   Examples by language:
   - Python: `test_user_service.py`
   - JavaScript: `user.service.test.js`
   - Go: `user_service_test.go`
   - Rust: `user_service_test.rs`
2. Write test stubs:
   - Test function signatures
   - Arrange-Act-Assert structure
   - Comments describing what to test
3. Import dependencies
4. Setup fixtures/mocks

**Artifacts:**
- Test files with stubs

**Gate:** Stub review — [A]ccept / [F]eedback / [R]eject

---

### Phase 4: IMPLEMENT

**Purpose:** Fill in test bodies.

**Steps:**
1. Implement unit tests:
   - Happy path scenarios
   - Error scenarios
   - Boundary conditions
2. Implement integration tests:
   - Component interaction
   - Data flow
3. Implement edge case tests:
   - From EDGE-CASES.md
   - Discovered during implementation
4. Add descriptive test names:
   - `test_calculate_total_with_discount()`
   - `test_reject_negative_quantity()`

**Artifacts:**
- Complete test files

**Gate:** Implementation review — [A]ccept / [F]eedback / [R]eject

---

### Phase 5: VALIDATE

**Purpose:** Run tests and fix failures.

**Steps:**
1. Run tests:
   ```bash
   # Language-specific command
   pytest tests/test_{feature}_*.py
   # or
   npm test -- test/{feature}
   # or
   go test ./...
   ```
2. Check results:
   - Pass/fail per test
   - Coverage percentage
3. Fix failures:
   - Debug failing tests
   - Fix code or test (whichever is wrong)
   - Re-run until passing
4. Check coverage:
   - Meet minimum threshold (if set)
   - Identify uncovered lines

**Artifacts:**
- Test results
- Coverage report
- Fixed code/tests

**Gate:** Validation review — [A]ccept / [F]eedback / [R]eject

---

### Phase 6: DOCUMENT

**Purpose:** Update docs with test info.

**Steps:**
1. Update TDD.md:
   - Testing approach used
   - Coverage achieved
   - Known gaps
2. Update issue file:
   - Mark tests as complete
   - Add test notes
3. Add to PATTERNS.md:
   - Testing patterns discovered

**Artifacts:**
- Updated documentation

**Gate:** [A]ccept / [F]eedback / [R]eject

---

## Test Naming Conventions

**Good:**
```python
def test_user_can_login_with_valid_credentials():
def test_login_rejects_empty_password():
def test_login_locks_after_three_failed_attempts():
```

**Bad:**
```python
def test1():
def test_login():
def test_stuff():
```

---

## Customization

Users can customize:
- Add test categories (performance, security)
- Change coverage threshold
- Add test data generators
- Add mocking frameworks
