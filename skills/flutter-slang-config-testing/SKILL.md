---
name: flutter-slang-config-testing
description: "Slang i18n: configuration reference, testing, CLI tools, tips"
---

# Slang — Config, Testing & CLI

## Configuration (slang.yaml)

All settings optional:

```yaml
base_locale: en
fallback_strategy: none
input_directory: lib/i18n
input_file_pattern: .i18n.json
output_directory: lib/i18n
output_file_name: strings.g.dart
lazy: true
locale_handling: true
flutter_integration: true
namespaces: false
translate_var: t
enum_name: AppLocale
class_name: Translations
translation_class_visibility: private
key_case: null # camel | pascal | snake | null
key_map_case: null
param_case: null
string_interpolation: dart
flat_map: true
translation_overrides: false
timestamp: true
statistics: true
sanitization:
enabled: true
prefix: k
case: camel
format:
enabled: false
width: 150
autodoc:
enabled: true
locales: [en]
obfuscation:
enabled: false
secret: null
```

## Testing

```dart
test('translations compile', () {
expect(AppLocale.en.build().aboutPage.title, 'About');
});

test('all locales supported by Flutter', () {
for (final locale in AppLocale.values) {
expect(kMaterialSupportedLanguages, contains(locale.languageCode));
}
});
```

## CLI Reference

```bash
# Generate
dart run slang
dart run slang watch

# Analysis
dart run slang analyze [--full] [--split]
dart run slang clean

# Edit
dart run slang edit add en key "value"
dart run slang edit move oldPath newPath
dart run slang edit copy oldPath newPath
dart run slang edit delete key
dart run slang edit outdated key

# Utilities
dart run slang normalize [--locale=fr]
dart run slang configure
dart run slang stats
dart run slang wip apply

# Migration
dart run slang migrate arb source.arb dest.json
```

## Tips

- **Auto-comments**: base locale shown as doc comment in generated code
- **Sanitization**: reserved keywords get `k` prefix (`continue` → `kContinue`)
- **Prototyping**: `t.$wip('Hello $name')` — then `dart run slang wip apply`
- **Comments in JSON**: `@key` matching a real key → doc comment
- **MCP Server**: `dart pub global activate slang_mcp` for LLM translations
