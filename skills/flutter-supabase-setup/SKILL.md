---
name: flutter-supabase-setup
description: Project setup and initialization for Supabase in Flutter. Use when Gemini CLI needs to configure the Supabase client or initialize the SDK in a Flutter app.
---

# Supabase Setup in Flutter

## Installation

Add `supabase_flutter` to `pubspec.yaml`:

```yaml
dependencies:
  supabase_flutter: ^2.0.0
```

## Initialization

Initialize Supabase in `main()` before calling `runApp()`.

```dart
import 'package:supabase_flutter/supabase_flutter.dart';

Future<void> main() async {
  await Supabase.initialize(
    url: 'https://xyz.supabase.co',
    anonKey: 'public-anon-key',
  );
  runApp(MyApp());
}
```

## Accessing the Client

Use the static instance to access the Supabase client anywhere in your app.

```dart
final supabase = Supabase.instance.client;
```

## Common Issues
- **Missing initialization:** Always ensure `Supabase.initialize` is awaited.
- **Deep Linking:** Required for OAuth and Magic Links. Set `authFlowType: AuthFlowType.pkce` in `initialize` for enhanced security.
