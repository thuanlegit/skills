---
name: flutter-slang-translations
description: "Slang i18n: string interpolation, lists, maps, RichText, linked translations, dynamic keys"
---

# Slang — Translation Features

## String Interpolation

Default mode is `dart` (`$name`, `${expr}`). Configure in slang.yaml:

```yaml
string_interpolation: dart # dart | braces | double_braces
```

```json
{ "hello": "Hello $name, you are ${age} years old" }
```

```dart
t.hello(name: 'Tom', age: 25);
```

## Typed Parameters

```json
{ "greet": "Hello {name: String}, you are {age: int} years old" }
```

## Lists

No config needed. Nested lists and maps inside lists supported:

```json
{
  "niceList": ["hello", "nice", ["nested item"], { "wow": "WOW!" }]
}
```

```dart
t.niceList[1];          // "nice"
t.niceList[2][0];       // "nested item"
t.niceList[3].wow;      // "WOW!"
```

## Maps (key-value access)

Add `(map)` modifier or configure in slang.yaml:

```json
{ "errors(map)": { "not_found": "Not found", "server": "Server error" } }
```

```yaml
maps:
  - errors
```

```dart
t.errors['not_found'];
```

## Dynamic Keys (Flat Map)

Access ANY translation by string path (enabled by default):

```dart
t['login.success'];
t['myList.0.title'];
t'hello';
```

Disable with `flat_map: false`.

## RichText

Add `(rich)` modifier. Parameters become `TextSpan`:

```json
{ "myText(rich)": "Welcome $name. Click ${tapHere(here)}!" }
```

```dart
Text.rich(t.myText(
  name: TextSpan(text: 'Tom', style: TextStyle(color: Colors.blue)),
  tapHere: (text) => TextSpan(
    text: text,
    style: TextStyle(color: Colors.blue),
    recognizer: TapGestureRecognizer()..onTap = () {},
  ),
));
```

## Linked Translations

Reference another translation with `@:path`:

```json
{
  "fields": { "name": "my name is {firstName}" },
  "intro": "Hello, @:fields.name and I am {age} years old"
}
```

```dart
t.intro(firstName: 'Tom', age: 27);
```

Escape paths with braces: `@:{fields.name}inator`
