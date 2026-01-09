# Lesson 3: Dart Basics (Part 2)

## Learning Goals

- Functions.
- Control Flow.
- Arrow Syntax.

## 1. Functions

Named parameters are standard in Flutter widgets.

```dart
// Positional
int add(int a, int b) {
  return a + b;
}

// Named (Optional by default, use 'required' to enforce)
void printUser({required String name, int? age}) {
  print('$name is $age');
}

// Usage
printUser(name: 'Alice', age: 30);
```

## 2. Arrow Syntax

Short functions for one-liners.

```dart
int multiply(int a, int b) => a * b;
```

## 3. Control Flow

Standard C-style syntax.

```dart
if (year >= 2000) {
  print('21st Century');
} else {
  print('20th Century');
}

for (var i = 0; i < 5; i++) {
  print(i);
}

// For-in (Iterables)
final list = [1, 2, 3];
for (final item in list) {
  print(item);
}
```

## Key Takeaways

- Flutter relies heavily on **Named Parameters** (`Widget(color: Colors.red)`).
- Understanding `required` and `?` (nullable) is crucial.
