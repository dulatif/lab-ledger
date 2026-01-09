# Lesson 4: Isolates (Concurrency)

## Learning Goals

- Main Thread vs Background Thread.
- `compute`.

## 1. The Main Thread

Flutter runs on a single UI Thread.
If you run `while(true)` or calculate Fibonacci(100), the app freezes (Jank).

## 2. compute()

Spawns a new Isolate (Thread-like), runs code, returns result, and kills it.

```dart
Future<int> heavyTask(int value) async {
  return await compute(_calculate, value);
}

// Must be a top-level or static function
int _calculate(int value) {
  // Expensive math
  return value * 2;
}
```

## 3. Isolate.spawn

For long-running background workers.
Isolates do _not_ share memory. They communicate via messages (Ports).

## Key Takeaways

- Use `compute` for heavy JSON parsing or Image processing.
- Never block the UI thread.
