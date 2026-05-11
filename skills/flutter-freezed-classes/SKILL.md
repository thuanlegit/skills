---
name: flutter-freezed-classes
description: Basic factory constructors, immutability, @Default, and copyWith (including deep copy) in Freezed. Use when creating basic data models.
---

# Flutter Freezed Classes

## Basic Usage

Freezed uses factory constructors to define immutable data classes.

```dart
@freezed
class User with _$User {
  const factory User({
    required String name,
    required int age,
    @Default(false) bool isAdmin, // Default value
  }) = _User;

  factory User.fromJson(Map<String, dynamic> json) => _$UserFromJson(json);
}
```

## copyWith & Deep Copy

Freezed automatically generates `copyWith`.

**Basic:** `var olderUser = user.copyWith(age: 26);`

**Deep Copy:** Access nested properties directly to avoid nested `copyWith` chains.
```dart
company.copyWith.director.assistant(name: 'New Name');
// For nullable nested objects:
company.copyWith.director.assistant?.call(name: 'New Name');
```
