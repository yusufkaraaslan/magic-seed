# Wizard: Deploy

> **Profile:** Unity Game  
> **Purpose:** Unity build pipeline and store submission

---

## When to Use

- Feature complete and merged
- Ready to build and deploy

## Input

- Merged feature
- Build configuration
- Platform targets

## Output

- Build artifacts
- Deployed release
- Store submission (if applicable)

---

## Phases

### Phase 1: CHECKLIST

Verify:
- [ ] All tests passing
- [ ] No compiler errors
- [ ] Scenes added to Build Settings
- [ ] Player Settings configured
- [ ] Version bumped

### Phase 2: VERSION

Bump version in:
- `ProjectSettings/ProjectSettings.asset`
- `CHANGELOG.md`
- Any version display UI

### Phase 3: BUILD

Build for target platforms:
```bash
# Example: Build for Windows
Unity -quit -batchmode -buildWindows64Player ./Builds/Windows/Game.exe

# Example: Build for Android
Unity -quit -batchmode -buildAndroid ./Builds/Android/Game.apk
```

### Phase 4: TEST BUILD

Run smoke tests on build:
- Launch game
- Test core loop
- Check for errors

### Phase 5: DEPLOY

Deploy to:
- Steam
- App Store / Google Play
- Itch.io
- WebGL hosting
- Internal distribution

### Phase 6: VERIFY

Confirm deployment success:
- Download and test
- Check analytics
- Monitor crash reports

### Phase 7: ANNOUNCE

Notify team and players:
- Release notes
- Social media
- Update server (if live ops)

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
