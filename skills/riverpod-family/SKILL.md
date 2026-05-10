---
name: riverpod-family
description: "Riverpod 3 family providers — passing arguments to providers."
version: 1.0.0
---

# Riverpod Family Providers (Code Generation)

Families let a provider accept parameters — like a `Map<K, Provider>` keyed by argument.

## Functional Provider with Args

```dart
@riverpod
Future<User> user(Ref ref, {required String id}) async {
  final response = await http.get(Uri.parse('/api/users/$id'));
  return User.fromJson(jsonDecode(response.body));
}
// Usage: ref.watch(userProvider(id: '123'))
```

### Positional args:

```dart
@riverpod
int add(Ref ref, int a, int b) => a + b;
// Usage: ref.watch(addProvider(1, 2))
```

### Optional args:

```dart
@riverpod
Future<List<Item>> search(Ref ref, {String query = '', int limit = 20}) async {
  // ...
}
// Usage: ref.watch(searchProvider(query: 'hello'))
```

## Notifier Provider with Args

```dart
@riverpod
class UserNotifier extends _$UserNotifier {
  @override
  Future<User> build({required String id}) async {
    final response = await http.get(Uri.parse('/api/users/$id'));
    return User.fromJson(jsonDecode(response.body));
  }

  Future<void> refresh() async {
    state = const AsyncLoading();
    state = await AsyncValue.guard(() async {
      // Access args via `arg` property
      final response = await http.get(Uri.parse('/api/users/${arg.id}'));
      return User.fromJson(jsonDecode(response.body));
    });
  }
}
// Usage:
// ref.watch(userNotifierProvider(id: '123'))
// ref.read(userNotifierProvider(id: '123').notifier).refresh()
```

## Key Rules

- Parameters go on the **function** (functional) or **build method** (notifier)
- Any number of params: positional, named, required, optional — all supported
- Each unique param combo creates an independent cached state
- Access notifier args via `arg` property (e.g., `arg.id`, `arg.page`)
- Family providers **should use auto-dispose** (default with `@riverpod`) to prevent memory leaks
