# Lesson 1: Implicit Animations

## Learning Goals

- `AnimatedContainer`.
- `AnimatedOpacity`.
- Curves.

## 1. The Concept

"Fire and Forget". You change the value, and Flutter interpolates the difference.

```dart
double opacity = 0.0;

// ... inside build ...
AnimatedOpacity(
  opacity: opacity,
  duration: Duration(seconds: 1),
  curve: Curves.easeInOut,
  child: Text('Hello'),
);

// Toggle
setState(() => opacity = 1.0);
```

## 2. AnimatedContainer

Can animate almost any styling property.

```dart
AnimatedContainer(
  duration: Duration(milliseconds: 500),
  width: isBig ? 200 : 100,
  color: isRed ? Colors.red : Colors.blue,
  child: child,
);
```

## Key Takeaways

- Use Implicit Animations for 90% of UI transitions.
- They are declarative and require no `AnimationController`.
