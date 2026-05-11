---
name: go-router-setup
description: "Initial setup, basic routing configuration, and core navigation methods for the go_router package. Use this when configuring the GoRouter instance, defining routes, handling parameters, or performing basic navigation (go/push)."
---

# go_router Setup & Basic Navigation

This skill covers the fundamental configuration and usage of the `go_router` package in Flutter.

## Core Workflow

1.  **Configuration**: Define a global `GoRouter` instance with a list of `GoRoute` objects.
2.  **Integration**: Pass the `GoRouter` instance to `MaterialApp.router` or `CupertinoApp.router` via the `routerConfig` property.
3.  **Navigation**: Use `context.go()` for declarative navigation (changing the URI) or `context.push()` for imperative navigation (adding to the stack).

## Key Patterns

- **Defining Routes**: Every route must have a `path` (starting with `/` for top-level routes).
- **Parameters**: Use template syntax in paths (e.g., `user/:id`) and access them via `state.pathParameters`.
- **Query Parameters**: Accessed via `state.uri.queryParameters`.
- **Named Routes**: Use the `name` property on `GoRoute` for easier navigation: `context.goNamed('user', pathParameters: {'id': '123'})`.

## Reference Materials

- [setup.md](references/setup.md): Boilerplate for initializing `GoRouter` and connecting it to `MaterialApp`.
- [navigation.md](references/navigation.md): Detailed comparison and examples of `go`, `push`, and named route navigation.
