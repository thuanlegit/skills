---
name: flutter-supabase-realtime
description: Realtime database subscriptions for Supabase in Flutter. Use when Gemini CLI needs to listen to Postgres changes (INSERT, UPDATE, DELETE) in real-time.
---

# Supabase Realtime in Flutter

## Subscribing to Changes

Listen to events on a specific table.

```dart
final channel = supabase.channel('public:messages');

channel.onPostgresChanges(
  event: PostgresChangeEvent.all, // .insert, .update, .delete
  schema: 'public',
  table: 'messages',
  callback: (payload) {
    print('Change received: ${payload.toString()}');
    // Payload contains 'new' and 'old' record data
  },
).subscribe();
```

## Unsubscribing
```dart
await supabase.removeChannel(channel);
```

## Note on Setup
Ensure **Realtime** is enabled for the table in the Supabase Dashboard (Database -> Replication -> supabase_realtime publication).
