# Authentication Guard Pattern

The most common use for redirection is ensuring users are authenticated before accessing certain parts of the app.

## Implementation Example

```dart
final _router = GoRouter(
  refreshListenable: authService, // authService is a ChangeNotifier
  redirect: (context, state) {
    final bool loggedIn = authService.isLoggedIn;
    final bool loggingIn = state.matchedLocation == '/login';

    if (!loggedIn) {
      // If not logged in and not on login page, redirect to login
      return loggingIn ? null : '/login';
    }

    if (loggingIn) {
      // If logged in and on login page, redirect to home
      return '/';
    }

    // No redirection needed
    return null;
  },
  routes: [
    GoRoute(path: '/', builder: ...),
    GoRoute(path: '/login', builder: ...),
    GoRoute(
      path: '/profile',
      builder: ...,
      // Optional: Route-level redirect
      redirect: (context, state) => checkPermissions(state) ? null : '/unauthorized',
    ),
  ],
);
```

## Important Considerations

- **State Sync**: Always use `refreshListenable` to ensure the router re-evaluates the redirect logic when your auth state changes (e.g., when the user clicks "Log Out").
- **Matched Location**: Use `state.matchedLocation` to check the current target URL to avoid infinite loops.
