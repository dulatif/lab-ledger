# Lesson 7: Styling & Flexbox

## Learning Goals

- Master the React Native Layout Engine (Flexbox).
- Understand Default Directions.

## 1. Flexbox Differences (Web vs Native)

1.  **flexDirection**: Defaults to `column` (Vertical) in React Native. Web defaults to `row`.
2.  **Display**: Everything is `display: flex` by default. `block` / `inline` do not exist.
3.  **Units**: Numbers are "Density Independent Pixels" (dp). `px`, `em`, `rem` do not exist.

## 2. Main Axis vs Cross Axis

If `flexDirection: 'column'` (Default):

- **Main Axis (Vertical)**: Controlled by `justifyContent`.
- **Cross Axis (Horizontal)**: Controlled by `alignItems`.

**Center Everything**:

```javascript
centered: {
    flex: 1, // Take up all space
    justifyContent: 'center', // Align on Main
    alignItems: 'center', // Align on Cross
}
```

## 3. Positioning

- **Relative**: Default. The element participates in the Flex flow.
- **Absolute**: Removed from flow. Position relative to the nearest parent.
  ```javascript
  floatingButton: {
      position: 'absolute',
      bottom: 20,
      right: 20,
  }
  ```

## 4. Shadow

Shadows are tricky.

- **iOS**: Uses `shadowColor`, `shadowOffset`, `shadowOpacity`.
- **Android**: Uses `elevation` (Material Design standard).

Usually, you define both in your style object to support both platforms.

## Key Takeaways

- React Native layouts are simpler than CSS grids.
- Everything is a Flex column by default.
- Use `elevation` for Android shadows.
