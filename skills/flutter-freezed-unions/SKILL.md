---
name: flutter-freezed-unions
description: Sealed classes and pattern matching for Freezed. Use when creating tagged unions and handling different states.
---

# Flutter Freezed Unions

## Unions (Sealed Classes)

Define multiple factory constructors to create a sealed class.

```dart
@freezed
sealed class Union with _$Union {
  const factory Union.data(int value) = Data;
  const factory Union.loading() = Loading;
  const factory Union.error([String? message]) = Error;
}
```

## Pattern Matching

Use Dart 3's `switch` or `if-case` to securely handle all states.

```dart
var state = Union.data(42);

switch (state) {
  case Data(value: final v):
    print('Data: $v');
  case Loading():
    print('Loading...');
  case Error(message: final m):
    print('Error: $m');
}
```
*Note: Shared properties must be defined in all constructors to be accessible directly on the main class.*
