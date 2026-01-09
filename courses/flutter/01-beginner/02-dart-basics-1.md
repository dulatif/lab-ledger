# Lesson 2: Dart Basics (Part 1)

## Learning Goals

- `main()` function.
- Variables (`var`, `final`, `const`).
- Built-in Types.

## 1. The Entry Point

Every Dart program starts with `main`.

```dart
void main() {
  print('Hello, Flutter!');
}
```

## 2. Variables & Type Inference

Dart is statically typed, but has type inference.

```dart
var name = 'Voyager I'; // Inferred as String
String specificName = 'Voyager II';
```

## 3. Immutability

Prefer immutability.

- `final`: Set once (runtime).
- `const`: Compile-time constant.

```dart
final date = DateTime.now(); // OK
// const date = DateTime.now(); // Error! DateTime.now() is runtime.
const double pi = 3.14159;
```

## 4. Built-in Types

- **Numbers**: `int`, `double`.
- **Strings**: `'Hello'` or `"Hello"`. Interpolation: `'Count: $count'`.
- **Booleans**: `bool` (true/false).
- **Lists**: `[1, 2, 3]`.

## Key Takeaways

- Always use `final` unless the variable _must_ change.
- Dart looks a lot like JavaScript but is Type Safe like Java.
