---
name: go-router-type-safe
description: "Implementation of compile-time safe routing using the go_router_builder package. Use this when you want to avoid string-based navigation and enforce strongly-typed parameters."
---

# go_router Type-Safe Routes

This skill covers how to use code generation to create a strongly-typed API for your application's routes.

## Core Workflow

1.  **Dependencies**: Add `go_router_builder` and `build_runner` to `dev_dependencies`.
2.  **Definition**: Define routes as classes extending `GoRouteData`.
3.  **Annotations**: Use `@TypedGoRoute` to map classes to paths.
4.  **Generation**: Run `dart run build_runner build` to generate the `.g.dart` file.
5.  **Navigation**: Use the generated `.go()` or `.push()` methods on the route classes.

## Key Patterns

- **Typed Parameters**: Defining fields in the route class automatically maps them to path/query parameters.
- **Nested Typed Routes**: Defining sub-routes using the `routes` property in the annotation.
- **Shell Routes**: Using `@TypedShellRoute` for nested navigation layouts.

## Reference Materials

- [typed_routes.md](references/typed_routes.md): Examples of defining typed routes and performing type-safe navigation.
