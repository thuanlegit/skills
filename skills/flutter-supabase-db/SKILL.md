---
name: flutter-supabase-db
description: Postgres database operations for Supabase in Flutter. Use when the agent needs to perform CRUD operations, filtering, or sorting on database tables.
---

# Supabase Database (CRUD)

## Select Data
```dart
// All rows
final data = await supabase.from('profiles').select();

// Filtered rows
final data = await supabase
    .from('profiles')
    .select()
    .eq('username', 'kiwi');

// Single row (throws if not exactly one)
final data = await supabase.from('profiles').select().single();
```

## Insert Data
```dart
await supabase.from('messages').insert({ 'content': 'Hello!' });
```

## Update Data
```dart
await supabase
    .from('profiles')
    .update({ 'username': 'new_kiwi' })
    .eq('id', 1);
```

## Delete Data
```dart
await supabase.from('messages').delete().eq('id', 1);
```

## Common Filters & Modifiers
- `.eq('column', value)`: Equals
- `.neq('column', value)`: Not equals
- `.in_('column', [value1, value2])`: In array
- `.like('column', '%pattern%')`: Pattern match
- `.order('column', ascending: false)`: Sorting
- `.limit(10)`: Pagination
