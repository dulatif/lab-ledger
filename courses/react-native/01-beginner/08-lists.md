# Lesson 8: Scrollable Lists (FlatList)

## Learning Goals

- Understand why `map()` is bad for scrolling.
- Implement `FlatList`.

## 1. The ScrollView

If you have a generic page with scrolling content (like a blog post), use `<ScrollView>`.
**Limitation**: It renders **all** children at once. If you have 1000 items, it will crash the app (memory overload).

## 2. FlatList (Virtualization)

`FlatList` is a "Virtual List". It only renders the items currently visible on the screen. As you scroll down, it recycles the memory.

**Anatomy**:

1.  **data**: An array of objects.
2.  **renderItem**: A function describing how to draw ONE row.
3.  **keyExtractor**: Unique ID for optimization.

```jsx
const DATA = [
  { id: "1", title: "First Item" },
  { id: "2", title: "Second Item" },
  // ... 100 more
];

const App = () => {
  return (
    <FlatList
      data={DATA}
      keyExtractor={(item) => item.id}
      renderItem={({ item }) => (
        <View style={styles.row}>
          <Text>{item.title}</Text>
        </View>
      )}
    />
  );
};
```

## 3. Common Props

- `horizontal`: Scroll sideways (like Netflix rows).
- `ItemSeparatorComponent`: Renders a line between items.
- `ListEmptyComponent`: Renders if `data` is empty.

## Key Takeaways

- Always use `FlatList` for lists of data (Arrays).
- Use `ScrollView` for mixed content (Header + Text + Button).
- **Performance**: Avoid creating anonymous functions inside `renderItem` if possible.
