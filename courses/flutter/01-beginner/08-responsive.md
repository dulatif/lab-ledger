# Lesson 8: Responsive Layouts

## Learning Goals

- `Expanded` & `Flexible`.
- `MediaQuery`.
- `Stack`.

## 1. Expanded vs Flexible

Used inside `Row` or `Column` to fill available space.

- `Expanded`: Forces child to fill empty space.
- `Flexible`: Lets child fill space but allows it to be smaller.

```dart
Row(
  children: [
    Icon(Icons.star), // Fixed width
    Expanded(child: Text('Very long text that wraps...')), // Fills remaining
  ],
);
```

## 2. Stack

Places widgets on top of each other (z-axis).
Use `Positioned` to anchor children.

```dart
Stack(
  children: [
    Image.asset('bg.jpg'),
    Positioned(
      bottom: 10,
      right: 10,
      child: Text('Watermark'),
    ),
  ],
);
```

## 3. MediaQuery

Get screen size.

```dart
final width = MediaQuery.of(context).size.width;
if (width > 600) {
  // Tablet Layout
}
```

## Key Takeaways

- Avoid hardcoding widths (e.g., `width: 300`). Use `Expanded` or percentages.
- Use `Stack` for overlays.
