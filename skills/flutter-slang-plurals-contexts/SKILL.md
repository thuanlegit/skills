---
name: flutter-slang-plurals-contexts
description: "Slang i18n: pluralization (cardinal/ordinal), custom contexts/enums (gender etc)"
---

# Slang — Pluralization & Custom Contexts

## Pluralization

Keywords: `zero`, `one`, `two`, `few`, `many`, `other`. Auto-detected (cardinals):

```json
{
  "apple": {
    "one": "I have $n apple.",
    "other": "I have $n apples."
  }
}
```

```dart
t.apple(n: 1); // "I have 1 apple."
t.apple(n: 2); // "I have 2 apples."
```

### Custom parameter name

```json
{
  "apple(param=appleCount)": {
    "one": "One apple",
    "other": "$appleCount apples"
  }
}
```

### Ordinal

```json
{
  "place(ordinal)": {
    "one": "${n}st place",
    "two": "${n}nd place",
    "few": "${n}rd place",
    "other": "${n}th place"
  }
}
```

### Config-based

```yaml
pluralization:
auto: off
default_parameter: n
cardinal: [someKey.apple]
ordinal: [someKey.place]
```

### Custom plural resolver

```dart
LocaleSettings.setPluralResolver(
locale: AppLocale.en,
cardinalResolver: (n, {zero, one, two, few, many, other}) {
if (n == 0) return zero ?? other!;
if (n == 1) return one ?? other!;
return other!;
},
);
```

## Custom Contexts / Enums

```json
{
  "greet(context=GenderContext)": {
    "male": "Hello Mr $name",
    "female": "Hello Ms $name"
  }
}
```

Generated: `enum GenderContext { male, female }`

```dart
t.greet(name: 'Maria', context: GenderContext.female);
```

### Shared form (collapse)

```json
{ "greet(context=GenderContext)": { "male,female": "Hello $name" } }
```

### Custom parameter name

```json
{
  "greet(context=GenderContext, param=gender)": {
    "male": "Hello Mr",
    "female": "Hello Ms"
  }
}
```

```dart
t.greet(gender: GenderContext.female);
```

### Use existing enum

```yaml
imports: ["package:my_app/my_enum.dart"]
contexts:
UserType:
generate_enum: false
```

## Multiple plurals in one sentence

```json
{
  "apples(param=appleCount)": {
    "one": "one apple",
    "other": "{appleCount} apples"
  },
  "bananas(param=bananaCount)": {
    "one": "one banana",
    "other": "{bananaCount} bananas"
  },
  "sentence": "I have @:apples and @:bananas"
}
```

```dart
t.sentence(appleCount: 1, bananaCount: 2);
```
