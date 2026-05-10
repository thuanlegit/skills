---
name: riverpod-testing
description: "Riverpod 3 testing: unit tests, widget tests, overrides, mocking."
version: 1.0.0
---

# Testing Riverpod Providers (Code Generation)

## Unit Tests (no Flutter)

```dart
import 'package:flutter_test/flutter_test.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';

void main() {
  test('counter starts at 0', () {
    final container = ProviderContainer.test();
    expect(container.read(counterProvider), 0);
  });

  test('counter can increment', () {
    final container = ProviderContainer.test();
    container.read(counterProvider.notifier).increment();
    expect(container.read(counterProvider), 1);
  });
}
```

**Always use `ProviderContainer.test()`** — auto-disposes after test.

## Override Providers (Mocking)

```dart
test('shows user from mocked provider', () {
  final container = ProviderContainer.test(
    overrides: [
      userProvider.overrideWith((ref) async => User(name: 'Mock')),
    ],
  );
});
```

## Widget Tests

```dart
testWidgets('displays counter value', (tester) async {
  await tester.pumpWidget(
    ProviderScope(
      overrides: [
        counterProvider.overrideWith((ref) => 42),
      ],
      child: const MaterialApp(home: CounterScreen()),
    ),
  );

  expect(find.text('42'), findsOneWidget);
});
```

## Testing Async Providers

```dart
test('user provider loads data', () async {
  final container = ProviderContainer.test(
    overrides: [
      userProvider.overrideWith((ref) async => User(name: 'Test')),
    ],
  );

  final user = await container.read(userProvider.future);
  expect(user.name, 'Test');
});
```

## Listen to Changes in Tests

```dart
test('tracks state changes', () {
  final container = ProviderContainer.test();
  final sub = container.listen(counterProvider, (prev, next) {});

  expect(sub.read(), 0);

  container.read(counterProvider.notifier).increment();
  expect(sub.read(), 1);
});
```

**Tip:** Use `container.listen` instead of `container.read` for auto-dispose providers — prevents premature disposal mid-test.
