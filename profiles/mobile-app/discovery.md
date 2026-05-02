# Discovery Guide: Mobile App

## Step 1: Framework

**Flutter:**
- `pubspec.yaml` exists
- `lib/main.dart`
- Framework version in `pubspec.yaml`

**React Native:**
- `package.json` with `react-native`
- `ios/`, `android/` directories
- `metro.config.js`

**Native iOS:**
- `*.xcodeproj`
- `*.swift` files

**Native Android:**
- `build.gradle`
- `*.kt` files

## Step 2: State Management

**Flutter:**
- Check `pubspec.yaml`:
  - `provider` → Provider
  - `flutter_riverpod` → Riverpod
  - `bloc` → BLoC
  - `mobx` → MobX
  - `get` → GetX

**React Native:**
- Check `package.json`:
  - `redux` → Redux
  - `zustand` → Zustand
  - `mobx` → MobX
  - `recoil` → Recoil

## Step 3: Navigation

**Flutter:**
- Check `pubspec.yaml`:
  - `go_router` → GoRouter
  - `auto_route` → AutoRoute
  - `flutter_navigation` → Navigator 2.0

**React Native:**
- Check `package.json`:
  - `@react-navigation/native` → React Navigation

## Step 4: Architecture

**Flutter:**
- Check `lib/` structure:
  - `screens/`, `widgets/`, `models/` → Simple
  - `tasks/`, `core/` → Feature-based
  - `presentation/`, `domain/`, `data/` → Clean Architecture

**React Native:**
- Check `src/` structure:
  - `screens/`, `components/` → Simple
  - `tasks/` → Feature-based

---

## Discovery Summary

```
Discovery Complete: Mobile App Profile

Project: {project-name}
  Framework: {framework}
  Platform: {iOS/Android/both}

State: {state-management}
Navigation: {navigation}

Architecture:
  Pattern: {architecture-pattern}
```
