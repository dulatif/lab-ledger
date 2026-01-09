# Lesson 1: Environment Setup

## Learning Goals

- Flutter CLI.
- Android Studio / Xcode.
- `flutter doctor`.

## 1. The Flutter CLI

Flutter is a command-line tool.
Download the SDK from [flutter.dev](https://flutter.dev).
Add `flutter/bin` to your system `PATH`.

```bash
flutter --version
```

## 2. Flutter Doctor

This is your best friend. It checks your system for missing dependencies.

```bash
flutter doctor
```

**Common fixes**:

- Install Android Studio cmdline-tools.
- Accept Android Licenses (`flutter doctor --android-licenses`).
- Install CocoaPods (for macOS/iOS).

## 3. Creating a Project

```bash
flutter create my_first_app
cd my_first_app
flutter run
```

## 4. The Counter App

The default app is a simple counter.

- `lib/main.dart`: The entry point.
- `pubspec.yaml`: Configuration (dependencies, assets).

## Key Takeaways

- If `flutter doctor` is all green, you are ready to code.
- Hot Reload (`r` in terminal) is Flutter's magic feature.
