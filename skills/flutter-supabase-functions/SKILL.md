---
name: flutter-supabase-functions
description: Invoking Supabase Edge Functions in Flutter. Use when Gemini CLI needs to call server-side logic (Deno functions) from the Flutter client.
---

# Supabase Edge Functions in Flutter

## Invoking a Function

Use `supabase.functions.invoke()` to call your Edge Function.

```dart
// Basic invocation
final res = await supabase.functions.invoke('hello-world');

// With body data
final res = await supabase.functions.invoke(
  'process-payment',
  body: {'amount': 100},
);

// Get response data
final data = res.data;
```

## Error Handling
```dart
try {
  await supabase.functions.invoke('fail-function');
} on FunctionException catch (e) {
  print('Status: ${e.status}');
  print('Error: ${e.details}');
}
```

## Headers
You can pass custom headers if needed:
```dart
await supabase.functions.invoke(
  'my-function',
  headers: {'X-Custom-Header': 'value'},
);
```
