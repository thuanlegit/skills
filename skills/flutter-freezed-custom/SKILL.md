---
name: flutter-freezed-custom
description: Private empty constructors for custom methods/getters and decorators (@Assert, @JsonKey) in Freezed. Use when adding logic or custom JSON keys to Freezed models.
---

# Flutter Freezed Custom Methods & Decorators

## Custom Methods & Getters

You must define a **private empty constructor** (`const ClassName._();`) to add logic to a Freezed class.

```dart
@freezed
class Person with _$Person {
  const Person._(); // Required for custom logic

  const factory Person(String name, {int? age}) = _Person;

  void sayHello() => print('Hello $name');
  bool get isAdult => (age ?? 0) >= 18;
}
```

## Decorators

*   **`@Default(value)`**: Default value for a parameter.
*   **`@Assert('condition', 'message')`**: Adds an assertion.
*   **`@JsonKey(name: '...')`**: Customizes JSON serialization.
*   **`@unfreezed`**: Use instead of `@freezed` to make properties mutable (non-final).
