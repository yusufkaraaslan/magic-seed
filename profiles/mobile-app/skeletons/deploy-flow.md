# Flow: Deploy

> **Profile:** Mobile App

## Phases

### Phase 1: CHECKLIST
- [ ] Tests pass
- [ ] Version bumped
- [ ] Icons/splash screens set
- [ ] Signing configured

### Phase 2: VERSION
Bump version in `pubspec.yaml` or `package.json`.

### Phase 3: BUILD
```bash
# Flutter
flutter build ios --release
flutter build appbundle --release

# React Native
npx react-native run-ios --configuration Release
npx react-native run-android --variant release
```

### Phase 4: TEST BUILD
Test on device.

### Phase 5: DEPLOY
Submit to:
- App Store (iOS)
- Google Play (Android)
- TestFlight (beta)
- Firebase App Distribution

### Phase 6: VERIFY
- [ ] App available
- [ ] Download works
- [ ] Core features work

### Phase 7: ANNOUNCE
Notify users.

## Platform Notes
- **iOS:** Requires Apple Developer account, Xcode
- **Android:** Requires keystore, Play Console
- **Beta:** Use TestFlight / Internal Testing first
