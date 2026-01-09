# Lesson 7: Reactivity

## Learning Goals

- `ValueNotifier`.
- `ChangeNotifier`.

## 1. ValueNotifier

The simplest form of state management.
It holds a single value. When it changes, it notifies listeners.

```dart
final count = ValueNotifier<int>(0);

// Update
count.value++;

// Listen
ValueListenableBuilder<int>(
  valueListenable: count,
  builder: (context, value, child) {
    return Text('$value');
  }
);
```

## 2. ChangeNotifier

Holds multiple values. You must manually call `notifyListeners()`.

```dart
class UserModel extends ChangeNotifier {
  String name = '';

  void setName(String newName) {
    name = newName;
    notifyListeners(); // Magic happens here
  }
}
```

## Key Takeaways

- Flutter is **Reactive**. You change the state -> The UI rebuilds.
- `ValueNotifier` is lightweight and built-in. Use it for local controller logic.
