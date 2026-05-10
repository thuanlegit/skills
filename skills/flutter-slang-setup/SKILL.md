---
name: flutter-slang-setup
description: "Slang i18n: setup, init, basic usage in Flutter"
---

# Slang — Setup & Basic Usage

Type-safe i18n for Flutter/Dart using JSON/YAML/CSV/ARB → generated Dart code.

## Install

```yaml
dependencies:
  slang: <version>
  slang_flutter: <version>

dev_dependencies:
  slang_build_runner: <version> # only if using build_runner
```

## Quick Start

**1. Create translation files** in `lib/i18n/`:

```
lib/i18n/
  en.i18n.json
  de.i18n.json
```

```json
// en.i18n.json
{
  "hello": "Hello $name",
  "login": {
    "success": "Logged in",
    "fail": "Login failed"
  }
}
```

**2. Generate code:**

```bash
dart run slang          # recommended
# OR
dart run build_runner build -d  # CI / initial checkout
```

**3. Initialize in Flutter:**

```dart
import 'package:my_app/i18n/strings.g.dart';

void main() {
  WidgetsFlutterBinding.ensureInitialized();
  LocaleSettings.useDeviceLocale();
  runApp(TranslationProvider(child: MyApp()));
}
```

```dart
MaterialApp(
  locale: TranslationProvider.of(context).flutterLocale,
  supportedLocales: AppLocaleUtils.supportedLocales,
  localizationsDelegates: GlobalMaterialLocalizations.delegates,
  child: YourFirstScreen(),
)
```

**4. Use translations:**

```dart
import 'package:my_app/i18n/strings.g.dart';

String a = t.login.success;
String b = t.hello(name: 'Tom');
```

## Set Specific Locale

```dart
LocaleSettings.setLocale(AppLocale.de);
LocaleSettings.setLocaleRaw('de');
LocaleSettings.useDeviceLocale();
```

## CLI Commands

```bash
dart run slang              # generate
dart run slang watch        # auto-rebuild on change
dart run slang analyze      # find missing/unused translations
dart run slang normalize    # sort keys to match base locale
dart run slang configure    # update iOS CFBundleLocalizations
```

## iOS: Info.plist

```xml
<key>CFBundleLocalizations</key>
<array>
   <string>en</string>
   <string>de</string>
</array>
```

Or run `dart run slang configure` to auto-update.
