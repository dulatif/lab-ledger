# Lesson 5: Testing Strategy

## Learning Goals

- Unit Tests.
- Widget Tests.
- Integration Tests.

## 1. Unit Tests

Test a pure Dart function or class. Fast.

```dart
test('counter increments', () {
  final counter = Counter();
  counter.increment();
  expect(counter.value, 1);
});
```

## 2. Widget Tests

Test a single component in a fake env.

```dart
testWidgets('MyWidget has a title', (tester) async {
  await tester.pumpWidget(MaterialApp(home: MyWidget(title: 'T')));

  final titleFinder = find.text('T');
  expect(titleFinder, findsOneWidget);
});
```

## 3. Integration Tests

Run on a real device/emulator. Click real buttons. Slow but reliable.

## Key Takeaways

- Aim for 70% Unit, 20% Widget, 10% Integration.
