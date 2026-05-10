---
name: riverpod-advanced
description: "Riverpod 3 advanced: auto-dispose, select, scoping, observers, retry, mutations, debounce."
version: 1.0.0
---

# Riverpod Advanced Features

## Auto-Dispose & KeepAlive

`@riverpod` is auto-dispose by default. State destroyed when no listeners remain.

```dart
// Default: auto-dispose
@riverpod
String tempValue(Ref ref) => 'gone soon';

// Keep alive: persists without listeners
@Riverpod(keepAlive: true)
String persistentValue(Ref ref) => 'stays forever';
```

React to disposal:

```dart
@riverpod
Stream<int> countdown(Ref ref) {
  final controller = StreamController<int>();
  ref.onDispose(() => controller.close());
  return controller.stream;
}
```

**Tip:** Family providers should always be auto-dispose to avoid memory leaks.

## Select — Reduce Rebuilds

Watch only a subset of a provider's state:

```dart
final name = ref.watch(userProvider.select((u) => u.firstName));
```

## Scoping — Override Per Subtree

```dart
// 1. Define with dependencies
@Riverpod(dependencies: [])
String? currentItemId(Ref ref) => null;

// 2. Override in ProviderScope for subtree
ProviderScope(
  overrides: [
    currentItemIdProvider.overrideWith((ref) => 'item-42'),
  ],
  child: ItemDetailScreen(),
)

// 3. Providers watching scoped ones must declare them
@Riverpod(dependencies: [currentItemId])
Future<Item> currentItem(Ref ref) async {
  final id = ref.watch(currentItemIdProvider);
  return fetchItem(id!);
}
```

## Observers — Logging/Analytics

```dart
class Logger extends ProviderObserver {
  @override
  void didUpdateProvider(context, prev, next) {
    print('${context.provider}: $prev → $next');
  }
}

void main() {
  runApp(ProviderScope(
    observers: [Logger()],
    child: MyApp(),
  ));
}
```

## Retry Logic

Auto-retry on failure (up to 10x, exponential backoff 200ms→6.4s).

```dart
Duration? myRetry(int count, Object error) {
  if (count >= 3) return null;
  return Duration(milliseconds: 200 * (1 << count));
}

@Riverpod(retry: myRetry)
int riskyOperation(Ref ref) => mightThrow();
```

Disable globally:

```dart
ProviderScope(retry: (_, __) => null, child: MyApp())
```

## Mutations (Experimental)

Track side-effect progress separate from provider state:

```dart
final addTodo = Mutation<Todo>();

// In widget:
final state = ref.watch(addTodo);

return switch (state) {
  MutationPending() => const CircularProgressIndicator(),
  MutationError(:final error) => Text('Error: $error'),
  _ => ElevatedButton(
    onPressed: () => addTodo.run(ref, (_) async => await api.addTodo(todo)),
    child: const Text('Add Todo'),
  ),
};
```

## Debounce/Cancel Requests

```dart
@riverpod
Future<List<Result>> search(Ref ref, {required String query}) async {
  final cancelToken = CancelToken();
  ref.onDispose(() => cancelToken.cancel());

  await Future.delayed(const Duration(milliseconds: 300));
  if (cancelToken.isCancelled) throw Exception('cancelled');

  return api.search(query, cancelToken: cancelToken);
}
```
