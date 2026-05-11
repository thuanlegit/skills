---
name: flutter-freezed-setup
description: Setup, installation, and build_runner for Flutter Freezed. Use when starting a new project with Freezed or needing the file header requirements.
---

# Flutter Freezed Setup

## Installation

```bash
# Add freezed and build_runner
flutter pub add dev:build_runner freezed_annotation dev:freezed
# Optional: Add json_serializable for JSON support
flutter pub add json_annotation dev:json_serializable
```

## Running the Generator

```bash
dart run build_runner watch -d
```

## File Header Requirements

Every file using Freezed must include:

```dart
import 'package:freezed_annotation/freezed_annotation.dart';

part 'filename.freezed.dart';
// Optional: if using JSON serialization
part 'filename.g.dart'; 
```
