# Lesson 9: Null Safety & OOP

## Learning Goals

- Sound Null Safety.
- Mixins.
- Abstract Classes.

## 1. Sound Null Safety

Variables cannot be null unless you say so.

```dart
String name = 'Bob'; // Never null
String? nickname = null; // Can be null

print(nickname?.length); // Safe access
print(nickname!.length); // Force unwrap (Dangerous: throws if null)
```

## 2. Mixins

Reuse code across unrelated classes. "Has-a" behavior.

```dart
mixin Swimmable {
  void swim() => print('Swimming...');
}

class Duck extends Bird with Swimmable {}
class Human extends Mammal with Swimmable {}
```

## 3. Abstract Interface

Dart doesn't have an `interface` keyword. Every class is an interface.
But we use `abstract class` to define contracts.

```dart
abstract class Repository {
  Future<void> save();
}

class SqlRepository implements Repository {
  @override
  Future<void> save() { ... }
}
```

## Key Takeaways

- Embrace Null Safety. It eliminates "Null Pointer Exceptions".
- Program to an Interface (Repository Pattern), not implementation.
