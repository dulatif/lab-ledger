# Lesson 4: Widgets 101

## Learning Goals

- The "Everything is a Widget" philosophy.
- `StatelessWidget`.
- `StatefulWidget`.

## 1. Everything is a Widget

In Flutter, the padding is a widget. The alignment is a widget. The text is a widget.
You build UI by composing small widgets into a tree.

## 2. Stateless Widget

Immutable. It receives configuration (props) and renders. It never changes.

```dart
class MyBadge extends StatelessWidget {
  final String label;

  const MyBadge({super.key, required this.label});

  @override
  Widget build(BuildContext context) {
    return Text(label);
  }
}
```

## 3. Stateful Widget

Mutable. It has a separate State object that persists across rebuilds.

```dart
class Counter extends StatefulWidget {
  const Counter({super.key});

  @override
  State<Counter> createState() => _CounterState();
}

class _CounterState extends State<Counter> {
  int count = 0;

  @override
  Widget build(BuildContext context) {
    return ElevatedButton(
      onPressed: () {
        setState(() {
          count++;
        });
      },
      child: Text('Count: $count'),
    );
  }
}
```

## Key Takeaways

- Use `StatelessWidget` by default.
- Use `StatefulWidget` only when you need `.setState()`.
