# Type-Safe Routing Implementation

Using `go_router_builder` eliminates typos in route names and ensures parameters are correctly typed.

## Step 1: Define the Routes

```dart
import 'package:flutter/material.dart';
import 'package:go_router/go_router.dart';

part 'routes.g.dart'; // Generated file

@TypedGoRoute<HomeRoute>(
  path: '/',
  routes: [
    TypedGoRoute<DetailsRoute>(path: 'details/:id'),
  ],
)
class HomeRoute extends GoRouteData {
  const HomeRoute();

  @override
  Widget build(BuildContext context, GoRouterState state) => const HomeScreen();
}

class DetailsRoute extends GoRouteData {
  final String id;
  const DetailsRoute({required this.id});

  @override
  Widget build(BuildContext context, GoRouterState state) => DetailsScreen(id: id);
}
```

## Step 2: Initialize GoRouter

```dart
final _router = GoRouter(
  // Use the generated $appRoutes top-level list
  routes: $appRoutes,
);
```

## Step 3: Type-Safe Navigation

Instead of `context.go('/details/123')`, use:

```dart
// Declarative
const DetailsRoute(id: '123').go(context);

// Imperative
const DetailsRoute(id: '123').push(context);

// With Query Parameters (just add them to the class constructor)
const SearchRoute(query: 'flutter').go(context);
```

## Benefits
- **Compile-time checks**: Errors are caught during development, not at runtime.
- **Auto-completion**: IDE provides suggestions for route classes and their required parameters.
- **Refactoring support**: Changing a route path or parameter name is much safer.
