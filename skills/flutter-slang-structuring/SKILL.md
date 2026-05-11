---
name: flutter-slang-structuring
description: "Slang i18n: namespaces, fallback, lazy loading, overrides, dependency injection"
---

# Slang — Structuring & Advanced

## Namespaces

Split translations into files per feature/domain:

```yaml
namespaces: true
output_directory: lib/i18n
output_file_name: strings.g.dart
```

Naming: `<namespace>_<locale>.<ext>`

```
i18n/
  widgets_en.i18n.json
  errorDialogs_en.i18n.json
```

```dart
t.widgets.welcomeCard.title;
t.errorDialogs.login.wrongPassword;
```

Root namespace: use `_default` as namespace name.

**Nested namespaces:** You can use multiple underscores for nested namespaces (e.g., `feature1_sub1_en.i18n.json`) to create a deeper tree structure.

## Fallback Strategy

```yaml
fallback_strategy: base_locale # none | base_locale | base_locale_empty_string
```

- `none` — all required (default)
- `base_locale` — missing keys fall back
- `base_locale_empty_string` — empty strings also treated as missing

Maps don't fallback by default. Add `(fallback)` modifier.

## Lazy Loading

Secondary locales loaded lazily by default. Disable:

```yaml
lazy: false
```

## Translation Overrides

Dynamic updates from backend:

```yaml
translation_overrides: true
```

```dart
LocaleSettings.overrideTranslations(
  locale: AppLocale.en,
  fileType: FileType.yaml,
  content: 'onboarding\\n  title: Welcome {name}',
);
```

## Dependency Injection (Riverpod)

```yaml
locale_handling: false
translation_class_visibility: public
```

```dart
final translationProvider = StateProvider<Translations>(
  (ref) => AppLocaleUtils.findDeviceLocale().build(),
);

final t = ref.watch(translationProvider);
t.welcome.title;

// MaterialApp
MaterialApp(
  locale: ref.watch(translationProvider).$meta.locale.flutterLocale,
  supportedLocales: AppLocaleUtils.supportedLocales,
);
```

## Multiple Packages

```dart
import 'package:pkg1/i18n/strings.g.dart' as pkg1;
import 'package:pkg2/i18n/strings.g.dart' as pkg2;

pkg1.LocaleSettings.setLocale(AppLocale.es); // syncs both packages
```

## Compact CSV

All locales in one file:

```csv
key,en,de-DE
welcome.title,Welcome $name,Willkommen $name
```
