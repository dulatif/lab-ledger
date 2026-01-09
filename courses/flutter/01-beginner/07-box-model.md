# Lesson 7: The Box Model & Flex

## Learning Goals

- `Container`.
- `Row` & `Column`.
- Main/Cross Axis.

## 1. Container

The Swiss Army Knife of layout.
Can handle: Size, Color, Border, Margin, Padding, Transform.

```dart
Container(
  width: 100,
  height: 100,
  padding: EdgeInsets.all(10),
  decoration: BoxDecoration(
    color: Colors.blue,
    borderRadius: BorderRadius.circular(8),
  ),
  child: Text('Box'),
);
```

## 2. Flex Layouts (Row & Column)

- **Row**: Horizontal. Main Axis = Horizontal.
- **Column**: Vertical. Main Axis = Vertical.

```dart
Column(
  mainAxisAlignment: MainAxisAlignment.center, // Vertical alignment
  crossAxisAlignment: CrossAxisAlignment.start, // Horizontal alignment
  children: [
    Text('Title'),
    Text('Subtitle'),
  ],
);
```

## Key Takeaways

- Compose complex layouts by nesting Rows and Columns.
- Use `SizedBox(height: 10)` for simple spacing.
