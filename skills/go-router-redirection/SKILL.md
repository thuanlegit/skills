---
name: go-router-redirection
description: "Implementation of authentication guards, global redirection logic, and custom error handling (404s) using go_router. Use this when you need to protect routes based on app state or define custom error pages."
---

# go_router Redirection & Error Handling

This skill covers how to control navigation flow based on application state (e.g., authentication) and how to handle routing errors.

## Core Workflow

1.  **Global Redirection**: Define a `redirect` callback at the `GoRouter` level. This runs on every navigation.
2.  **Route-Level Redirection**: Define a `redirect` callback on specific `GoRoute` entries for fine-grained control.
3.  **Refreshing**: Use `refreshListenable` (often with a `ChangeNotifier`) to trigger the redirect logic whenever specific state changes.
4.  **Error Handling**: Use `errorBuilder` to provide a custom UI for invalid URLs or routing failures.

## Key Patterns

- **Auth Guard**: Checking if a user is logged in before allowing access to private routes.
- **Circular Redirects**: Ensure your logic doesn't redirect from `/login` back to `/login`.
- **Error Screens**: Providing a user-friendly "Page Not Found" screen.

## Reference Materials

- [auth_guard.md](references/auth_guard.md): Standard pattern for protecting routes based on an authentication service.
- [error_handling.md](references/error_handling.md): Configuration for custom error pages and handling exceptions.
