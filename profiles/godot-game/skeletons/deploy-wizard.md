# Wizard: Deploy

> **Profile:** Godot Game

## When to Use
Feature complete and merged.

## Phases

### Phase 1: CHECKLIST
- [ ] Export presets configured
- [ ] Version bumped
- [ ] No errors

### Phase 2: VERSION
Update version in `project.godot`.

### Phase 3: BUILD
Export for target platforms:
```bash
godot --export-release "Windows Desktop" ./builds/windows/game.exe
godot --export-release "Linux/X11" ./builds/linux/game.x86_64
godot --export-release "Mac OSX" ./builds/mac/game.zip
```

### Phase 4: TEST BUILD
Run smoke tests.

### Phase 5: DEPLOY
Deploy to:
- Steam
- Itch.io
- Google Play
- App Store

### Phase 6: VERIFY
Confirm deployment success.

### Phase 7: ANNOUNCE
Notify team/players.

## Platform Notes
- **Desktop:** Single executable
- **Mobile:** Export templates required
- **Web:** HTML5 export
- **Console:** Requires platform SDK
