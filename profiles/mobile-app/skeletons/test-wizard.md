# Wizard: Test

> **Profile:** Mobile App

## When to Use
After implement-wizard.

## Test Examples

```dart
// Flutter widget test
testWidgets('Counter increments', (tester) async {
  await tester.pumpWidget(MyApp());
  await tester.tap(find.byIcon(Icons.add));
  await tester.pump();
  expect(find.text('1'), findsOneWidget);
});

// Integration test
test('login flow', () async {
  app.main();
  await tester.pumpAndSettle();
  
  await tester.enterText(find.byType(TextField).first, 'user@example.com');
  await tester.tap(find.byType(ElevatedButton));
  await tester.pumpAndSettle();
  
  expect(find.text('Welcome!'), findsOneWidget);
});
```

## Customization
Add golden tests, performance tests.
