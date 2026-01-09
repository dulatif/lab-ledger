# Lesson 7: Analytics & Crashlytics

## Learning Goals

- Firebase Analytics.
- Crash Reporting.

## 1. Analytics

Log events to understand user behavior.

```dart
FirebaseAnalytics.instance.logEvent(
  name: 'purchase',
  parameters: {'item': 'sword', 'price': 100},
);
```

## 2. Crashlytics

Auto-catch fatal errors.

```dart
FlutterError.onError = FirebaseCrashlytics.instance.recordFlutterFatalError;
```

Log non-fatal errors manually:

```dart
try {
  // ...
} catch (e, stack) {
  FirebaseCrashlytics.instance.recordError(e, stack);
}
```

## Key Takeaways

- You cannot fix bugs you don't know about.
- Crashlytics is essential for production apps.
