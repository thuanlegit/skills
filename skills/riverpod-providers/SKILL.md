---
name: riverpod-providers
description: "Riverpod 3 provider types with riverpod_generator."
version: 1.0.0
---

# Riverpod Provider Types (Code Generation)

## Provider Selection Guide

Don't think "which provider" — think "what do I return":

| Return type | Functional (read-only)            | Notifier (mutable)                  |
| ----------- | --------------------------------- | ----------------------------------- |
| `T` (sync)  | `@riverpod T fn(Ref ref)`         | `extends _$N` + `T build()`         |
| `Future<T>` | `@riverpod Future<T> fn(Ref ref)` | `extends _$N` + `Future<T> build()` |
| `Stream<T>` | `@riverpod Stream<T> fn(Ref ref)` | `extends _$N` + `Stream<T> build()` |

Use **functional** for computed/derived values. Use **notifier** when widgets need to mutate state.

---

## Functional Providers (read-only)

### Sync

```dart
@riverpod
String greeting(Ref ref) => 'Hello world';
// Usage: ref.watch(greetingProvider)
```

### Future (async)

```dart
@riverpod
Future<User> user(Ref ref) async {
  final response = await http.get(Uri.parse('/api/user/123'));
  return User.fromJson(jsonDecode(response.body));
}
// Usage: ref.watch(userProvider).whenData((user) => ...)
```

### Stream

```dart
@riverpod
Stream<int> countdown(Ref ref) => Stream.periodic(
  const Duration(seconds: 1), (i) => 10 - i,
);
```

## Notifier Providers (mutable)

### Notifier (sync state)

```dart
@riverpod
class Counter extends _$Counter {
  @override
  int build() => 0;

  void increment() => state++;
  void decrement() => state--;
  void reset() => state = 0;
}
// Usage: ref.watch(counterProvider)             → read state
//        ref.read(counterProvider.notifier).increment()  → mutate
```

### AsyncNotifier (async state)

```dart
@riverpod
class UserNotifier extends _$UserNotifier {
  @override
  Future<User> build(String id) async {
    final response = await http.get(Uri.parse('/api/user/$id'));
    return User.fromJson(jsonDecode(response.body));
  }

  Future<void> refresh() async {
    state = const AsyncLoading();
    state = await AsyncValue.guard(() async {
      final response = await http.get(Uri.parse('/api/user/${arg}'));
      return User.fromJson(jsonDecode(response.body));
    });
  }
}
// Usage: ref.watch(userNotifierProvider(id))
//        ref.read(userNotifierProvider(id).notifier).refresh()
```

**Key rules:**

- Class must extend `_$ClassName` (generated base class)
- `build()` returns initial state (sync or async)
- Access family args via `arg` property (e.g., `arg.id`)
- Access `ref` via `this.ref` anywhere in the class
- **Never** put logic in constructor — use `build()` instead
