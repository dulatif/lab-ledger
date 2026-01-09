# Lesson 5: Material & Cupertino

## Learning Goals

- The `Scaffold`.
- Material Design (Android).
- Cupertino (iOS).

## 1. The Scaffold

The basic structure of a visual page.
Provides slots for `appBar`, `body`, `floatingActionButton`, `drawer`.

```dart
Scaffold(
  appBar: AppBar(title: Text('Home')),
  body: Center(child: Text('Content')),
  floatingActionButton: FloatingActionButton(onPressed: () {}),
);
```

## 2. Material Widgets

Flutter's default design system.

- `ElevatedButton`, `TextButton`, `OutlinedButton`.
- `Card`, `ListTile`.
- `TextField`.

## 3. Cupertino Widgets

iOS-style widgets. Import `package:flutter/cupertino.dart`.

- `CupertinoPageScaffold`.
- `CupertinoButton`.
- `CupertinoSwitch`.

## Key Takeaways

- You can mix them, but usually, you stick to Material (which adapts slightly to iOS) or use a platform-aware wrapper.
