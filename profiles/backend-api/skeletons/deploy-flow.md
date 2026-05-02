# Flow: Deploy

> **Profile:** Backend API

## Phases

This profile follows the canonical **2-phase, 2-CRITICAL-gate** structure from `universal/workflow-structure.md` (and `profiles/generic/skeletons/deploy-flow.md`). Backend-API deploy specifics below.

### Phase 1: PREPARE *(CRITICAL gate)*
- **1.1 CHECKLIST** — Tests pass (pytest), database migrations ready, environment vars set, Docker image builds, no debug code or secrets in artifact.
- **1.2 VERSION** — Bump version; update CHANGELOG.md; generate release notes.
- **1.3 BUILD** — Build artifact + smoke test:
  ```bash
  docker build -t myapp:{version} .
  ```
- → **Gate 1: Ready to deploy?** — last reversible point. Sub-task 2.1 (deploy) executes only after this gate is accepted.

### Phase 2: SHIP *(CRITICAL gate)*
- **2.1 DEPLOY** *(executes only after Gate 1 is accepted)* — Push to target host: Kubernetes, AWS ECS/Fargate, Google Cloud Run, Azure Container Instances, Heroku, VPS.
  - Run database migrations BEFORE app start (or use blue/green for zero-downtime).
  - Keep previous version ready for rollback.
- **2.2 VERIFY** — Health checks:
  - [ ] Health check endpoint passes
  - [ ] API responds (200 on canary requests)
  - [ ] Database connected
  - [ ] Logs normal (no error spike)
  - Hold for soak window if profile rules require one.
  - If anything regresses → roll back; do not proceed to 2.3.
- **2.3 ANNOUNCE** *(executes only after Gate 2 is accepted)* — Notify team via configured channels.
- → **Gate 2: All clear?** — announcement is **public**. Roll back instead of accept if metrics regressed.

## Platform Notes
- **Blue/Green:** Zero-downtime deployment
- **Database:** Run migrations before app start
- **Rollback:** Keep previous version ready
