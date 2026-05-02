# Flow: Deploy

> **Profile:** Unity Game  
> **Purpose:** Unity build pipeline and store submission

---

## When to Use

- Task complete and merged
- Ready to build and deploy

## Input

- Merged task
- Build configuration
- Platform targets

## Output

- Build artifacts
- Deployed release
- Store submission (if applicable)

---

## Phases

This profile follows the canonical **2-phase, 2-CRITICAL-gate** structure from `universal/workflow-structure.md` (and `profiles/generic/skeletons/deploy-flow.md`). Unity-specific build/deploy details below.

### Phase 1: PREPARE *(CRITICAL gate)*
- **1.1 CHECKLIST** — All tests passing (EditMode + PlayMode), no compiler errors, scenes added to Build Settings, Player Settings configured, no debug code, no test scenes in build.
- **1.2 VERSION** — Bump version in `ProjectSettings/ProjectSettings.asset`, `CHANGELOG.md`, and any version display UI; generate release notes.
- **1.3 BUILD** — Build for target platforms; smoke test each artifact (launch game, test core loop, check for errors):
  ```bash
  # Windows
  Unity -quit -batchmode -buildWindows64Player ./Builds/Windows/Game.exe

  # Android
  Unity -quit -batchmode -buildAndroid ./Builds/Android/Game.apk
  ```
- → **Gate 1: Ready to deploy?** — last reversible point. Sub-task 2.1 (deploy) executes only after this gate is accepted.

### Phase 2: SHIP *(CRITICAL gate)*
- **2.1 DEPLOY** *(executes only after Gate 1 is accepted)* — Push to target platform(s): Steam, App Store / Google Play, Itch.io, WebGL hosting, internal distribution.
  - Mobile: check permissions, verify IAP configuration, test on physical device (not just emulator).
  - Console: follow platform guidelines, submit for certification.
  - WebGL: optimize for web, check browser compatibility, handle memory limits.
- **2.2 VERIFY** — Health checks:
  - Download and test from store listing
  - Check analytics
  - Monitor crash reports
  - If anything regresses → halt rollout / unpublish; do not proceed to 2.3.
- **2.3 ANNOUNCE** *(executes only after Gate 2 is accepted)* — Notify team and players (release notes, social media, live ops server update).
- → **Gate 2: All clear?** — announcement is **public**. Halt rollout instead of accept if metrics regressed.

---

## Platform-Specific Notes

### Mobile (iOS/Android)
- Check permissions
- Verify IAP configuration
- Test on device (not just emulator)

### Console (Switch/PS/Xbox)
- Follow platform guidelines
- Submit for certification
- Handle platform-specific input

### WebGL
- Optimize for web
- Check browser compatibility
- Handle memory limits

---

## Customization

Add:
- CI/CD integration
- Automated build pipeline
- Beta testing workflow
- Live ops deployment
