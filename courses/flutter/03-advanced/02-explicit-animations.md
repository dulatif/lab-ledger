# Lesson 2: Explicit Animations

## Learning Goals

- `AnimationController`.
- `Tween`.
- `AnimatedBuilder`.

## 1. AnimationController

Requires `SingleTickerProviderStateMixin`. It drives the animation (0.0 to 1.0).

```dart
class _MyState extends State<MyWidget> with SingleTickerProviderStateMixin {
  late AnimationController _controller;

  @override
  void initState() {
    super.initState();
    _controller = AnimationController(
      vsync: this,
      duration: Duration(seconds: 2)
    );
    _controller.repeat();
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }
}
```

## 2. Tween

Mapping the 0.0-1.0 value to something useful (Colors, Offsets).

```dart
final animation = Tween<Offset>(
  begin: Offset.zero,
  end: Offset(100, 0)
).animate(_controller);
```

## 3. AnimatedBuilder

Rebuilds only the specific part of the widget tree when valid.

```dart
AnimatedBuilder(
  animation: _controller,
  builder: (context, child) {
    return Transform.translate(
      offset: animation.value,
      child: child, // Optimization: child is built once
    );
  },
  child: Container(width: 50, height: 50, color: Colors.red),
)
```

## Key Takeaways

- Use Explicit animations when you need control (pause, reverse, repeat, sequence).
