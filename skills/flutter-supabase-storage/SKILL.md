---
name: flutter-supabase-storage
description: File storage operations for Supabase in Flutter. Use when the agent needs to upload, download, or manage files and buckets in Supabase Storage.
---

# Supabase Storage in Flutter

## File Operations

### Upload File
```dart
import 'dart:io';

final File file = File('path/to/file.png');
await supabase.storage.from('avatars').upload('public/avatar.png', file);
```

### Download File
```dart
final Uint8List file = await supabase.storage.from('avatars').download('public/avatar.png');
```

### Delete File
```dart
await supabase.storage.from('avatars').remove(['public/avatar.png']);
```

## URLs

### Get Public URL
```dart
final String publicUrl = supabase.storage.from('avatars').getPublicUrl('public/avatar.png');
```

### Create Signed URL
```dart
final String signedUrl = await supabase.storage
    .from('avatars')
    .createSignedUrl('public/avatar.png', 60); // valid for 60 seconds
```

## Bucket Management
```dart
// List buckets
final List<Bucket> buckets = await supabase.storage.listBuckets();
```
