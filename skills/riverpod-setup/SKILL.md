---
name: riverpod-setup
description: "Riverpod 3 setup with riverpod_generator for Flutter projects."
---

# Riverpod 3 Setup (Code Generation)

## Install

```bash
flutter pub add flutter_riverpod riverpod_annotation
flutter pub add dev:riverpod_generator dev:build_runner dev:custom_lint dev:riverpod_lint
```

## Wrap App with ProviderScope

```dart
void main() {
  runApp(
    ProviderScope(child: MyApp()),
  );
}
```

## Project Structure

Organize by **feature** rather than by type:
- `features/auth/state/auth_provider.dart` (Recommended)
- `providers/auth_provider.dart` (Legacy/Small apps)

Keep logic pure: Providers should only manage state. Avoid putting navigation or UI-specific code inside a provider.

## Run Code Generation

```bash
# One-shot build
dart run build_runner build

# Watch mode (rebuild on change)
dart run build_runner watch

# Clean + rebuild
dart run build_runner build --delete-conflicting-outputs
```

## File Convention

Every file using `@riverpod` must include:

```dart
import 'package:riverpod_annotation/riverpod_annotation.dart';

part 'my_file.g.dart';
```

The generated file `my_file.g.dart` creates:

- `myFunctionProvider` for `@riverpod` on function `myFunction`
- `myNotifierProvider` for `@riverpod` on class `MyNotifier`

## Annotations

- `@riverpod` — auto-dispose (default, recommended)
- `@Riverpod(keepAlive: true)` — persists even when no listeners
