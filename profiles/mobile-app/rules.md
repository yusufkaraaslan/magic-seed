# Rules: Mobile App

## Architecture Rules

### Rule M1: Platform Abstraction

Isolate platform-specific code.

```dart
// Flutter: Platform channel abstraction
abstract class BatteryService {
  Future<int> getBatteryLevel();
}

class BatteryServiceImpl implements BatteryService {
  @override
  Future<int> getBatteryLevel() async {
    // Platform-specific implementation
  }
}
```

### Rule M2: Screen/Page Architecture

Each screen is self-contained.

```dart
// Flutter
class HomeScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Home')),
      body: HomeBody(),
    );
  }
}
```

## Code Style

### Rule M3: Flutter Conventions

- **PascalCase** for classes, enums
- **camelCase** for variables, functions
- **lowercase_with_underscores** for file names
- **UPPER_SNAKE_CASE** for constants

```dart
const MAX_RETRY_COUNT = 3;
var userName = 'John';

class UserProfile extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Container();
  }
}
```

### Rule M4: React Native Conventions

- **PascalCase** for components
- **camelCase** for functions, variables
- **UPPER_SNAKE_CASE** for constants

## Performance

### Rule M5: Avoid Rebuilds (Flutter)

Use `const` constructors and `ValueKey`.

```dart
// ✓ Good
const ListTile(
  leading: Icon(Icons.person),
  title: Text('User'),
);

// ✗ Bad
ListTile(
  leading: Icon(Icons.person),
  title: Text('User'),
);
```

### Rule M6: Lazy Load Screens

Load screens on demand.

```dart
// Flutter: Lazy load routes
GoRoute(
  path: '/details',
  builder: (context, state) => const DetailsScreen(),
)
```

## Testing

### Rule M7: Widget/Component Tests

```dart
// Flutter
testWidgets('Counter increments', (tester) async {
  await tester.pumpWidget(MyApp());
  await tester.tap(find.byIcon(Icons.add));
  await tester.pump();
  expect(find.text('1'), findsOneWidget);
});
```

### Rule M8: Integration Tests

Test full user flows.

```dart
// Flutter integration test
test('login flow', () async {
  app.main();
  await tester.pumpAndSettle();
  
  await tester.enterText(find.byType(TextField).first, 'user@example.com');
  await tester.tap(find.byType(ElevatedButton));
  await tester.pumpAndSettle();
  
  expect(find.text('Welcome!'), findsOneWidget);
});
```

## Validation Checklist

- [ ] Platform-specific code isolated
- [ ] Screens are self-contained
- [ ] Naming conventions followed
- [ ] Const constructors used (Flutter)
- [ ] Screens lazy loaded
- [ ] Widget/component tests present
- [ ] Integration tests for flows
- [ ] Accessibility labels added
- [ ] Error handling for network requests
- [ ] Loading states implemented
