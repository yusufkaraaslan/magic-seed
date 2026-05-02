# Flow: Deploy

> **Profile:** Mobile App

## Phases

This profile follows the canonical **2-phase, 2-CRITICAL-gate** structure from `universal/workflow-structure.md` (and `profiles/generic/skeletons/deploy-flow.md`). Mobile-app deploy specifics below.

### Phase 1: PREPARE *(CRITICAL gate)*
- **1.1 CHECKLIST** — Tests pass, version bumped, icons/splash screens set, signing configured (iOS keystore + Apple Developer account; Android keystore + Play Console).
- **1.2 VERSION** — Bump version in `pubspec.yaml` or `package.json`; update CHANGELOG.md; generate release notes.
- **1.3 BUILD** — Build for target platforms; test on physical device:
  ```bash
  # Flutter
  flutter build ios --release
  flutter build appbundle --release

  # React Native
  npx react-native run-ios --configuration Release
  npx react-native run-android --variant release
  ```
- → **Gate 1: Ready to deploy?** — last reversible point. Sub-task 2.1 (submit) executes only after this gate is accepted.

### Phase 2: SHIP *(CRITICAL gate)*
- **2.1 DEPLOY** *(executes only after Gate 1 is accepted)* — Submit to: App Store (iOS), Google Play (Android), TestFlight (beta), Firebase App Distribution.
  - Best practice: submit to beta channel first (TestFlight / Internal Testing), then production after soak.
- **2.2 VERIFY** — Health checks:
  - [ ] App listing visible in store
  - [ ] Download works
  - [ ] Core features work on device
  - [ ] Crash reports clean
  - If anything regresses → halt rollout / unpublish; do not proceed to 2.3.
- **2.3 ANNOUNCE** *(executes only after Gate 2 is accepted)* — Notify users via configured channels.
- → **Gate 2: All clear?** — announcement is **public**. Halt rollout instead of accept if metrics regressed.

## Platform Notes
- **iOS:** Requires Apple Developer account, Xcode
- **Android:** Requires keystore, Play Console
- **Beta:** Use TestFlight / Internal Testing first
