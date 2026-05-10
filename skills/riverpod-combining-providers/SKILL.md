---
name: riverpod-combining-providers
description: "Riverpod 3 combining providers: dependency chains."
---

# Combining Providers (Code Generation)

Providers can depend on other providers using `ref.watch`. When a dependency changes, the dependent provider automatically re-computes.

## Functional: Depend on Other Providers

```dart
@riverpod
int price(Ref ref) => 100;

@riverpod
int discountedPrice(Ref ref) {
  final p = ref.watch(priceProvider);
  return (p * 0.9).round();
}
```

## Notifier: Depend on Other Providers

```dart
@riverpod
class FilteredTodos extends _$FilteredTodos {
  @override
  List<Todo> build() {
    final todos = ref.watch(todosProvider);
    final filter = ref.watch(filterProvider);

    return switch (filter) {
      Filter.all => todos,
      Filter.completed => todos.where((t) => t.done).toList(),
      Filter.active => todos.where((t) => !t.done).toList(),
    };
  }
}
```

## Async Depends on Async

```dart
@riverpod
Future<String> userCity(Ref ref) async {
  // Use .future to await an async provider inside another provider
  final user = await ref.watch(userProvider.future);
  return user.city;
}
```

## Don't Call ref.watch in Methods

```dart
@riverpod
class MyNotifier extends _$MyNotifier {
  @override
  int build() {
    // ✅ ref.watch is OK here
    ref.watch(otherProvider);
    return 0;
  }

  void doSomething() {
    // ❌ NEVER ref.watch in methods — only ref.read
    final val = ref.read(otherProvider);
  }
}
```

**Rule:** `ref.watch` only in `build()` or functional provider bodies. `ref.read` everywhere else.

## Invalidate to Force Refresh

```dart
// From inside notifier:
ref.invalidateSelf();

// From outside:
ref.invalidate(userProvider);
ref.invalidate(userProvider(id: '123'));
```
