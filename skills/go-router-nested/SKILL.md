---
name: go-router-nested
description: "Implementation of nested navigation and persistent UI elements (like BottomNavigationBar) using ShellRoute and StatefulShellRoute. Use this when building dashboard layouts or tabbed interfaces."
---

# go_router Nested Navigation

This skill covers how to implement persistent UI elements that remain visible while navigating through sub-screens.

## Core Workflow

1.  **ShellRoute**: Used for basic nested navigation. It wraps a child navigator with a common UI (the "shell"). State is not automatically preserved when switching between sibling routes.
2.  **StatefulShellRoute**: Preferred for Tab-based navigation (like a `BottomNavigationBar`). It creates separate navigators for each branch, preserving the state of each tab automatically.
3.  **IndexedStack**: Often used within the shell's `builder` to switch between branches.

## Key Patterns

- **Bottom Navigation**: Implementing a persistent bottom bar.
- **Side Drawers**: Implementing a shell that includes a side navigation drawer.
- **Nested Routes**: Defining sub-routes that share the same shell.

## Reference Materials

- [stateful_shell_route.md](references/stateful_shell_route.md): Complete boilerplate for a `StatefulShellRoute` with a `BottomNavigationBar`.
