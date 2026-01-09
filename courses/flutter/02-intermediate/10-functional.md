# Lesson 10: Functional Concepts

## Learning Goals

- Collections methods.
- Immutability.

## 1. List Transformations

Declarative code is better than imperative loops.

```dart
final numbers = [1, 2, 3, 4, 5];

final squares = numbers
    .where((n) => n.isEven) // Filter
    .map((n) => n * n)      // Transform
    .toList();              // Materialize
```

## 2. Spread Operator

Combine lists easily.

```dart
final a = [1, 2];
final b = [3, 4];
final combined = [...a, ...b]; // [1, 2, 3, 4]
```

## 3. Cascades

Perform multiple operations on the same object.

```dart
final paint = Paint()
  ..color = Colors.black
  ..strokeWidth = 5.0
  ..style = PaintingStyle.stroke;
```

## Key Takeaways

- Use `map`/`where` instead of `for` loops when transforming data.
- Cascades (`..`) make builder-style configuration readable.
