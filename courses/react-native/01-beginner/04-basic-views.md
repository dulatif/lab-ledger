# Lesson 4: Basic Views & Layout

## Learning Goals

- Master the `View` component.
- Handle safe areas (Notch / Dynamic Islands).

## 1. The `<View>`

The fundamental building block (like `<div>`).
It supports nesting, styling, and touch handling (basic).

```jsx
<View style={{ flex: 1, backgroundColor: "white" }}>
  <View style={{ height: 50, backgroundColor: "blue" }} />
</View>
```

## 2. SafeAreaView

Mobile phones have notches, status bars, and rounded corners. Use `SafeAreaView` from `react-native-safe-area-context` (or `react-native` core, though libraries are preferred) to ensure content isn't hidden.

```jsx
import { SafeAreaView, Text } from "react-native";

const App = () => {
  return (
    <SafeAreaView style={{ flex: 1 }}>
      <Text>This text won't be under the notch!</Text>
    </SafeAreaView>
  );
};
```

- **Note**: `SafeAreaView` only works on iOS natively. Android requires extra setup or libraries usually handled by Expo Router.

## 3. StyleSheet

Don't use inline styles object literals `{{ }}` everywhere. It creates a new object on every render (performance hit).

```jsx
import { StyleSheet, View } from "react-native";

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 20,
    backgroundColor: "#fff",
  },
  box: {
    width: 100,
    height: 100,
  },
});

// Usage
<View style={styles.container}>...</View>;
```

## Key Takeaways

- **Flex: 1**: This makes a view expand to fill all available space. Without it, the View might have height 0.
- **StyleSheet**: Use it for better performance and cleaner code.
