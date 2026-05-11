# Persistent Tabs with StatefulShellRoute

`StatefulShellRoute` is the standard way to implement a `BottomNavigationBar` where each tab maintains its own navigation stack and scroll position.

## Implementation Example

```dart
final _router = GoRouter(
  initialLocation: '/home',
  routes: [
    StatefulShellRoute.indexedStack(
      builder: (context, state, navigationShell) {
        // Return the widget that implements the custom shell (e.g., BottomNav)
        return ScaffoldWithNavBar(navigationShell: navigationShell);
      },
      branches: [
        // Tab 1: Home
        StatefulShellBranch(
          routes: [
            GoRoute(
              path: '/home',
              builder: (context, state) => const HomeScreen(),
              routes: [
                GoRoute(path: 'details', builder: ...) // Pushed inside Home tab
              ],
            ),
          ],
        ),
        // Tab 2: Settings
        StatefulShellBranch(
          routes: [
            GoRoute(
              path: '/settings',
              builder: (context, state) => const SettingsScreen(),
            ),
          ],
        ),
      ],
    ),
  ],
);

// The Shell Widget
class ScaffoldWithNavBar extends StatelessWidget {
  final StatefulNavigationShell navigationShell;

  const ScaffoldWithNavBar({super.key, required this.navigationShell});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: navigationShell,
      bottomNavigationBar: BottomNavigationBar(
        currentIndex: navigationShell.currentIndex,
        onTap: (index) => navigationShell.goBranch(index),
        items: const [
          BottomNavigationBarItem(icon: Icon(Icons.home), label: 'Home'),
          BottomNavigationBarItem(icon: Icon(Icons.settings), label: 'Settings'),
        ],
      ),
    );
  }
}
```

## Key Benefits
- **State Preservation**: Moving from Home to Settings and back to Home keeps the Home tab's scroll position and nested stack.
- **Declarative**: Branches are defined clearly in the route tree.
- **NavigationShell**: The `navigationShell` object handles the logic for switching between branches.
