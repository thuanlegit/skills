---
name: flutter-supabase-auth
description: User authentication and session management for Supabase in Flutter. Use when Gemini CLI needs to implement sign-up, sign-in, auth state listening, or sign-out logic.
---

# Supabase Auth in Flutter

## Authentication Methods

### Email & Password
```dart
// Sign Up
final AuthResponse res = await supabase.auth.signUp(
  email: 'example@email.com',
  password: 'password',
);

// Sign In
final AuthResponse res = await supabase.auth.signInWithPassword(
  email: 'example@email.com',
  password: 'password',
);
```

### Magic Link (OTP)
```dart
await supabase.auth.signInWithOtp(email: 'example@email.com');
```

### Social Auth (OAuth)
```dart
await supabase.auth.signInWithOAuth(OAuthProvider.google);
```

## Listening to Auth State

Use `onAuthStateChange` to react to login/logout events (e.g., in a `StatefulWidget`).

```dart
supabase.auth.onAuthStateChange.listen((data) {
  final AuthChangeEvent event = data.event;
  final Session? session = data.session;
  // Update UI or state based on event
});
```

## Sign Out
```dart
await supabase.auth.signOut();
```

## User Data
```dart
final user = supabase.auth.currentUser;
final email = user?.email;
```
