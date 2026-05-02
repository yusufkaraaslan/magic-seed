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

This flow has **2 phases, 2 CRITICAL gates** — both gates are CRITICAL because each represents a hard-to-reverse decision (going live; declaring success). Sub-tasks within each phase auto-proceed; the gates are deliberately preserved at the two highest-stakes transitions. See `universal/workflow-structure.md`.

---

### Phase 1: PREPARE

**Purpose:** Verify the build is ready to ship. This phase produces nothing externally visible — the gate is the last reversible point.

**Sub-tasks (auto-proceed):**

**1.1 CHECKLIST**
- Verify task complete: all task flows closed, PR merged, tests passing
- Verify documentation: `task-design.md` signed off, `task-technical-design.md` complete, lessons learned captured
- Verify build: no build errors, dependencies resolved
- Verify safety: no debug code in build, no secrets in artifacts

**1.2 VERSION**
- Determine version bump: major (breaking) / minor (new tasks) / patch (bug fixes)
- Update version files: `package.json`, `pyproject.toml`, `Cargo.toml`, `version` file (whichever applies)
- Update `CHANGELOG.md`:
  ```markdown
  ## [1.2.0] - {date}

  ### Added
  - Task: {task-name}

  ### Changed
  - {other changes}

  ### Fixed
  - {bug fixes}
  ```
- Generate release notes

**1.3 BUILD**
- Run build command (per profile):
  ```bash
  npm run build              # JavaScript/TypeScript
  python setup.py build      # Python
  cargo build --release      # Rust
  make build                 # Make
  ```
- Verify build succeeds
- Check artifact size, format, signatures
- Run smoke tests on build artifact

**Artifacts (presented at gate):**
- Checklist report (pass/fail per item)
- Updated version files + CHANGELOG.md
- Release notes
- Build artifacts (size, signature, smoke test result)

**Gate:** Ready to deploy? — [A]ccept / [F]eedback / [R]eject  
*(GATE TYPE C — CRITICAL)*

⚠️ This is the **last reversible point**. Sub-task 2.1 (the actual deploy) executes only after this gate is accepted.

**Failure modes to verify before [A]ccept:**
- All checklist items pass?
- Version number correct (no accidental bump direction)?
- Build artifact sane (size / format / contents)?
- Rollback plan documented?

---

### Phase 2: SHIP

**Purpose:** Deploy, verify health, and announce. Each sub-task is irreversible in escalating ways — DEPLOY is irreversible-with-rollback; ANNOUNCE is publicly irreversible.

**Sub-tasks (auto-proceed):**

**2.1 DEPLOY** *(executes only after Phase 1 gate is accepted)*
- Choose environment: staging / production / other
- Run deployment:
  ```bash
  npm run deploy:staging
  # or
  docker-compose up -d
  # or
  kubectl apply -f k8s/
  # or
  rsync -avz dist/ server:/var/www/
  ```
- Monitor deployment, check for errors

**2.2 VERIFY**
- Health checks: application responds, database connections work, APIs return 200
- Task verification: new task works in production, existing features still work
- Performance check: response times acceptable, no memory leaks
- Monitoring: error rates normal, no alerts triggered
- Hold for soak window if profile rules require one
- If anything regresses → roll back per the rollback procedure below; do not proceed to 2.3

**2.3 ANNOUNCE** *(executes only after Phase 2 gate is accepted)*
- Compose announcement:
  ```
  Release {version} is live!

  What's new:
  - {task-name}: {brief description}

  Full changelog: {link}
  ```
- Send to team channels (Slack, Discord, email)
- Update status page (if applicable)
- Commit changelog updates separately

**Artifacts (presented at gate):**
- Deployment confirmation + logs
- Health check results
- Performance metrics vs baseline
- Soak window status (if applicable)
- Draft announcement text

**Gate:** All clear? — [A]ccept / [F]eedback / [R]ollback  
*(GATE TYPE C — CRITICAL)*

⚠️ Announcement is **public**. Roll back instead of [A]ccept if metrics regressed.

**Failure modes to verify before [A]ccept:**
- Health checks all green for the soak window?
- Error rate within tolerance vs baseline?
- Anyone reporting issues out-of-band?
- Release notes accurate and link to changelog?

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
   - Fix task flow

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
