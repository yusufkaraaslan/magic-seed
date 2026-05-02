# Flow: Deploy

> **Profile:** Web Frontend

## Phases

This profile follows the canonical **2-phase, 2-CRITICAL-gate** structure from `universal/workflow-structure.md` (and `profiles/generic/skeletons/deploy-flow.md`). Web-frontend deploy specifics below.

### Phase 1: PREPARE *(CRITICAL gate)*
- **1.1 CHECKLIST** — All task flows closed, tests pass (jest/vitest), build succeeds, environment variables set, no debug code in build.
- **1.2 VERSION** — Bump version in `package.json`; update CHANGELOG.md; generate release notes.
- **1.3 BUILD** — Build with project tooling; smoke test the artifact:
  ```bash
  npm run build    # or yarn build / pnpm build
  ```
- → **Gate 1: Ready to deploy?** — last reversible point. Sub-task 2.1 (deploy) executes only after this gate is accepted.

### Phase 2: SHIP *(CRITICAL gate)*
- **2.1 DEPLOY** *(executes only after Gate 1 is accepted)* — Push to target host: Vercel, Netlify, AWS S3 + CloudFront, GitHub Pages, Firebase Hosting, etc.
- **2.2 VERIFY** — Health checks:
  - [ ] Site loads
  - [ ] Routes work
  - [ ] API calls succeed
  - [ ] Assets load
  - SPA: redirect rules correct. SSR: server starts. Static: CDN cache invalidated.
  - If anything regresses → roll back; do not proceed to 2.3.
- **2.3 ANNOUNCE** *(executes only after Gate 2 is accepted)* — Notify team via configured channels.
- → **Gate 2: All clear?** — announcement is **public**. Roll back instead of accept if metrics regressed.

## Platform Notes
- **SPA:** Configure redirect rules
- **SSR:** Check server startup
- **Static:** Verify CDN cache invalidation
