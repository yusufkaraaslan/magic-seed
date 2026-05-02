# Flow: Deploy

> **Profile:** Godot Game

## When to Use
Task complete and merged.

## Phases

This profile follows the canonical **2-phase, 2-CRITICAL-gate** structure from `universal/workflow-structure.md` (and `profiles/generic/skeletons/deploy-flow.md`). Godot-specific export details below.

### Phase 1: PREPARE *(CRITICAL gate)*
- **1.1 CHECKLIST** — All task flows closed, tests pass (gdUnit4/GUT), export presets configured, no errors in editor.
- **1.2 VERSION** — Bump version in `project.godot`; update CHANGELOG.md; generate release notes.
- **1.3 BUILD** — Export for target platforms; smoke test each artifact:
  ```bash
  godot --export-release "Windows Desktop" ./builds/windows/game.exe
  godot --export-release "Linux/X11" ./builds/linux/game.x86_64
  godot --export-release "Mac OSX" ./builds/mac/game.zip
  ```
- → **Gate 1: Ready to deploy?** — last reversible point. Sub-task 2.1 (deploy) executes only after this gate is accepted.

### Phase 2: SHIP *(CRITICAL gate)*
- **2.1 DEPLOY** *(executes only after Gate 1 is accepted)* — Push to target platform(s): Steam, Itch.io, Google Play, App Store.
- **2.2 VERIFY** — Confirm deployment success (download counts, crash reports, store listing accurate). If anything regresses → roll back; do not proceed to 2.3.
- **2.3 ANNOUNCE** *(executes only after Gate 2 is accepted)* — Notify team/players via configured channels.
- → **Gate 2: All clear?** — announcement is **public**. Roll back instead of accept if metrics regressed.

## Platform Notes
- **Desktop:** Single executable
- **Mobile:** Export templates required
- **Web:** HTML5 export
- **Console:** Requires platform SDK
