# go_router Navigation Methods

## context.go() vs context.push()

It is critical to understand the difference between these two methods as they affect the navigation stack differently.

| Method | Behavior | Use Case |
| :--- | :--- | :--- |
| `context.go(location)` | **Declarative**. Navigates to the location based on the URI. It may discard the existing stack depending on the route tree. | Main navigation flow, deep linking, switching between top-level sections. |
| `context.push(location)` | **Imperative**. Pushes a new page onto the existing navigator stack. | Opening a detail page or modal where you want a back button to return to the exact previous state. |

## Examples

### Using Strings
```dart
context.go('/details/123');
context.push('/settings?theme=dark');
```

### Using Named Routes
Named routes are preferred to avoid hardcoding URI strings throughout the app.

```dart
// Definition
GoRoute(
  name: 'user_details',
  path: 'user/:id',
  builder: ...,
)

// Navigation
context.goNamed(
  'user_details',
  pathParameters: {'id': '42'},
  queryParameters: {'tab': 'profile'},
);
```

### Returning Values
`context.push` returns a `Future`, which completes when the pushed page is popped.

```dart
final result = await context.push<String>('/input-page');
// handle result
```
