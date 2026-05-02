# Flow: Deploy

> **Profile:** Generic  
> **Purpose:** Prepare and execute deployment or release

---

## When to Use

- Task is complete and merged
- Ready to deploy to staging/production
- Preparing a release

## Input

- Completed task (merged to main)
- Version number (or auto-generate)
- Deployment config

## Output

- Deployed release
- Verification report
- Updated changelogs

## Prerequisites

- All tests passing
- Documentation complete
- Task signed off

---

## Phases

### Phase 1: CHECKLIST

**Purpose:** Verify all prerequisites.

**Steps:**
1. Verify task complete:
   - [ ] All task flows closed
   - [ ] PR merged
   - [ ] Tests passing
2. Verify documentation:
   - [ ] task-design.md signed off
   - [ ] task-technical-design.md complete
   - [ ] Lessons learned captured
3. Verify build:
   - [ ] No build errors
   - [ ] Dependencies resolved
4. Verify version:
   - [ ] Version number determined
   - [ ] Changelog updated

**Artifacts:**
- Checklist report

**Gate:** [A]ccept / [F]ix issues / [R]eject

---

### Phase 2: VERSION

**Purpose:** Bump version and update changelogs.

**Steps:**
1. Determine version bump:
   - Major (breaking changes)
   - Minor (new tasks)
   - Patch (bug fixes)
2. Update version files:
   - `package.json` (npm)
   - `pyproject.toml` (Python)
   - `Cargo.toml` (Rust)
   - `version` file (generic)
3. Update `CHANGELOG.md`:
   ```markdown
   ## [1.2.0] - 2026-04-23
   
   ### Added
   - Task: {task-name}
   
   ### Changed
   - {other changes}
   
   ### Fixed
   - {bug fixes}
   ```
4. Update release notes

**Artifacts:**
- Updated version files
- Updated CHANGELOG.md

**Gate:** [A]ccept / [F]eedback / [R]eject

---

### Phase 3: BUILD

**Purpose:** Generate build artifacts.

**Steps:**
1. Run build command (examples for various build systems):
   ```bash
   # JavaScript/TypeScript
   npm run build
   # Python
   python setup.py build
   # Rust
   cargo build --release
   # Make
   make build
   ```
2. Verify build succeeds
3. Check build artifacts
4. Run smoke tests on build

**Artifacts:**
- Build artifacts
- Build log

**Gate:** [A]ccept / [F]ix issues / [R]eject

---

### Phase 4: DEPLOY

**Purpose:** Deploy to target environment.

**Steps:**
1. Choose environment:
   - Staging
   - Production
   - Other
2. Run deployment:
   ```bash
   # Examples
   npm run deploy:staging
   # or
   docker-compose up -d
   # or
   kubectl apply -f k8s/
   # or
   rsync -avz dist/ server:/var/www/
   ```
3. Monitor deployment
4. Check for errors

**Artifacts:**
- Deployment confirmation
- Logs

**Gate:** [A]ccept / [R]ollback / [R]eject

---

### Phase 5: VERIFY

**Purpose:** Confirm deployment success.

**Steps:**
1. Health checks:
   - Application responds
   - Database connections work
   - APIs return 200
2. Task verification:
   - New task works in production
   - Existing features still work
3. Performance check:
   - Response times acceptable
   - No memory leaks
4. Monitoring:
   - Error rates normal
   - No alerts triggered

**Artifacts:**
- Verification report

**Gate:** [A]ccept / [I]ssue found / [R]ollback

---

### Phase 6: ANNOUNCE

**Purpose:** Notify team/users.

**Steps:**
1. Write announcement:
   ```
   Release {version} is live!
   
   What's new:
   - {task-name}: {brief description}
   
   Full changelog: {link}
   ```
2. Send to team channels (Slack, Discord, email)
3. Update status page (if applicable)

**Artifacts:**
- Announcement sent

**Gate:** Complete

---

## Rollback Procedure

If verification fails:

1. **Immediate:**
   - Revert deployment
   - Restore previous version
   - Verify rollback success

2. **Investigation:**
   - Check logs
   - Identify root cause
   - Fix issue

3. **Retry:**
   - Re-deploy after fix
   - Verify again

---

## Customization

Users can customize:
- Add deployment environments (QA, canary)
- Add approval gates
- Add automated smoke tests
- Add notification channels
- Add database migration steps
