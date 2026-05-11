# Custom Error Handling

`go_router` provides an easy way to handle 404s (Page Not Found) or other routing-related errors.

## errorBuilder Configuration

```dart
final _router = GoRouter(
  routes: [...],
  errorBuilder: (context, state) => ErrorScreen(
    error: state.error,
  ),
);

// Example Error Screen
class ErrorScreen extends StatelessWidget {
  final Exception? error;
  const ErrorScreen({super.key, this.error});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Column(
          children: [
            const Text('404 - Page Not Found'),
            Text(error?.toString() ?? 'Unknown error'),
            ElevatedButton(
              onPressed: () => context.go('/'),
              child: const Text('Go Home'),
            ),
          ],
        ),
      ),
    );
  }
}
```

## When `errorBuilder` is Triggered
- Navigating to a path that isn't defined in the `routes` list.
- Failures during path parameter parsing.
- Exceptions thrown within a `redirect` callback.
