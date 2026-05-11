---
name: riverpod-consumers
description: "Riverpod 3 widget integration: ConsumerWidget, ref.watch/read/listen."
---

# Riverpod Consumers — Widgets That Use Providers

## ConsumerWidget (replaces StatelessWidget)

```dart
class HomeScreen extends ConsumerWidget {
  const HomeScreen({super.key});

  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final counter = ref.watch(counterProvider);
    return Text('Count: $counter');
  }
}
```

## ConsumerStatefulWidget (replaces StatefulWidget)

```dart
class HomeScreen extends ConsumerStatefulWidget {
  const HomeScreen({super.key});

  @override
  ConsumerState<HomeScreen> createState() => _HomeScreenState();
}

class _HomeScreenState extends ConsumerState<HomeScreen> {
  @override
  void initState() {
    super.initState();
    // ref is available here in ConsumerState
  }

  @override
  Widget build(BuildContext context) {
    final counter = ref.watch(counterProvider);
    return Text('Count: $counter');
  }
}
```

## Consumer (builder pattern — wrap any widget)

```dart
class MyWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Consumer(
      builder: (context, ref, child) {
        final value = ref.watch(myProvider);
        return Text('$value');
      },
    );
  }
}
```

## Ref Methods

### ref.watch(provider) — Reactive read (rebuilds on change)

Use in `build()` and provider functions. Triggers rebuild when value changes.

### ref.read(provider) — One-time read (no rebuild)

Use in event handlers, `initState`, callbacks. Does NOT listen for changes.

```dart
ElevatedButton(
  onPressed: () => ref.read(counterProvider.notifier).increment(),
  child: Text('Add'),
)
```

### ref.listen(provider, callback) — Fire-and-forget side effect

Use for snackbar, dialog, navigation. Does NOT rebuild.

```dart
ref.listen(userProvider, (previous, next) {
  if (next.hasError) {
    ScaffoldMessenger.of(context).showSnackBar(
      SnackBar(content: Text('Error: ${next.error}')),
    );
  }
});
```

## AsyncValue Pattern (for Future/Stream providers)

```dart
final asyncValue = ref.watch(userProvider);

return switch (asyncValue) {
  AsyncData(:final value) => Text(value.name),
  AsyncError(:final error) => Text('Error: $error'),
  _ => const CircularProgressIndicator(),
};

// Or with .when():
ref.watch(userProvider).when(
  data: (user) => Text(user.name),
  loading: () => const CircularProgressIndicator(),
  error: (err, stack) => Text('Error: $err'),
);

---

## Best Practices

- **Keep Logic Pure**: Providers should manage state and logic. Avoid putting navigation, snackbars, or UI-specific code inside a provider.
- **Avoid ref.read in build**: Use `ref.watch` in `build`. Use `ref.read` only in event handlers or lifecycle methods.
- **Select for Performance**: Use `.select` to watch only the properties that affect a specific widget's build.
```
