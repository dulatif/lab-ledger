# Lesson 7: Performance Optimization

## Learning Goals

- Understand the JS Thread bottleneck.
- Optimize Lists (FlashList).
- Use `React.memo` and `useCallback`.

## 1. The Bottleneck

Logic runs on the JS Thread. UI updates run on the Main (UI) Thread.
Messages pass over the Bridge.
If JS is doing heavy calculations (filtering 10k items), the Bridge clogs -> UI freezes.

## 2. List Optimization

`FlatList` is good, but **FlashList** (by Shopify) is 5x faster (and plug-and-play).

```bash
npm install @shopify/flash-list
```

**Usage**: It recycles components more aggressively.

- **Requirement**: You must specify `estimatedItemSize` (e.g., 50 pixels tall). This lets it calculate scroll height before rendering.

## 3. Memoization

Prevent re-renders of children when Parent state changes.

```jsx
// Child
const ExpensiveComponent = React.memo(({ data }) => {
  return <ComplexGraph data={data} />;
});

// Parent
const App = () => {
  // useCallback guarantees this function object stays the same reference
  const handlePress = useCallback(() => console.log("Hi"), []);

  return <Button onPress={handlePress} />;
};
```

## 4. Hermes Engine

A JS engine optimized for React Native (Android default).

- Starts up faster.
- Smaller APK size.
- Always enable it in `app.json`.

## Key Takeaways

- **FlashList**: Use it for long lists.
- **Hermes**: Ensure it is enabled.
- **Profiler**: Use Flipper or React DevTools to find "Why did this render?".
