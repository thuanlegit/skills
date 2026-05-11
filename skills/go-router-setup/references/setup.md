# go_router Setup Boilerplate

## Basic Initialization

```dart
import 'package:flutter/material.dart';
import 'package:go_router/go_router.dart';

// 1. Define the GoRouter configuration
final _router = GoRouter(
  initialLocation: '/',
  routes: [
    GoRoute(
      path: '/',
      builder: (context, state) => const HomeScreen(),
    ),
    GoRoute(
      path: '/details/:itemId',
      builder: (context, state) {
        final itemId = state.pathParameters['itemId']!;
        return DetailsScreen(itemId: itemId);
      },
    ),
  ],
);

// 2. Connect to MaterialApp
class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp.router(
      routerConfig: _router,
      title: 'GoRouter Example',
    );
  }
}
```

## Handling Parameters

### Path Parameters
Defined in the path template: `/users/:userId`.
Access: `state.pathParameters['userId']`.

### Query Parameters
URL: `/search?q=flutter`.
Access: `state.uri.queryParameters['q']`.
