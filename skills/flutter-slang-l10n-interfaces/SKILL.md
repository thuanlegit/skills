---
name: flutter-slang-l10n-interfaces
description: "Slang i18n: L10n formatting (dates, numbers, currency), interfaces, modifiers reference"
---

# Slang — L10n Formatting & Interfaces

## L10n (Dates & Numbers)

Uses `intl` package. Specify type after colon:

```json
{
  "greet": "Hello {name: String}, you have {amount: currency}",
  "today": "Today is {date: yMd}",
  "time": "It is {t: jm}"
}
```

```dart
t.greet(name: 'Tom', amount: 1234.56);  // "Hello Tom, you have $1,234.56"
t.today(date: DateTime(2023, 12, 31));   // "Today is 12/31/2023"
```

### Built-in types

**Numbers:** `compact`, `compactCurrency`, `compactLong`, `currency`, `decimalPattern`, `decimalPercentPattern`, `percentPattern`, `scientificPattern`, `simpleCurrency`

**Dates:** `yM`, `yMd`, `Hm`, `Hms`, `jm`, `jms`

### Custom formats

```json
{
  "today": "Today is {date: DateFormat('yyyy-MM-dd')}",
  "price": "Costs {p: currency(symbol: 'EUR')}"
}
```

### Reusable types (@@types)

```json
{
  "@@types": {
    "price": "currency(symbol: 'USD')",
    "dateOnly": "DateFormat('MM/dd/yyyy')"
  },
  "account": "You have {amount: price}",
  "today": "Today is {d: dateOnly}"
}
```

## Interfaces

Share structure between multiple translation maps:

```json
{
  "onboarding": {
    "whatsNew(interface=ChangeData)": {
      "v2": { "title": "New in 2.0", "rows": ["Add sync"] },
      "v3": { "title": "New in 3.0", "rows": ["New modes", "More!"] }
    }
  }
}
```

Generated mixin:

```dart
mixin ChangeData {
  String get title;
  List<String> get rows;
}
```

Usage:

```dart
void showChange(ChangeData change) {
  print(change.title);
  print(change.rows);
}
showChange(t.onboarding.whatsNew.v2);
```

### Config attributes

```yaml
# slang.yaml
interfaces:
  PageData:
    generate_mixin: true # true (default) generates a new mixin
    attributes:
      - String title
      - String? content
```

Set `generate_mixin: false` if you want to use a manually defined mixin (must be imported in `imports`).

*Note: Specifying paths for interfaces in `slang.yaml` is deprecated; use the `(interface=...)` modifier directly on keys.*

## Modifier Reference

| Modifier                  | Use                              |
| ------------------------- | -------------------------------- |
| `(rich)`                  | RichText (TextSpan params)       |
| `(map)`                   | Dictionary / key-value access    |
| `(fallback)`              | Fallback map keys to base locale |
| `(plural)` / `(cardinal)` | Cardinal pluralization           |
| `(ordinal)`               | Ordinal pluralization            |
| `(context=Type)`          | Custom enum context              |
| `(param=Name)`            | Custom parameter name            |
| `(interface=I)`           | Container of interfaces          |
| `(singleInterface=I)`     | Single interface node            |
| `(ignoreMissing)`         | Skip in analysis                 |
| `(ignoreUnused)`          | Skip in analysis                 |
| `(OUTDATED)`              | Flag for re-translation          |

Combine: `"apple(cardinal, param=cnt, rich)": { ... }`
