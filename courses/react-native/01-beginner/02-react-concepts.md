# Lesson 2: React Core Concepts

## Learning Goals

- Functional Components.
- Props vs State.
- The `useState` Hook.

## 1. Functional Components

A component is just a function that returns JSX (UI structure).

```jsx
import React from "react";
import { Text, View } from "react-native";

const Welcome = () => {
  return (
    <View>
      <Text>Hello, World!</Text>
    </View>
  );
};
```

## 2. Props (Read-Only)

Props allow you to pass data **down** from Parent to Child. They are immutable.

```jsx
// Child
const Greeting = ({ name }) => {
  return <Text>Hello, {name}!</Text>;
};

// Parent
const App = () => {
  return (
    <View>
      <Greeting name="Alice" />
      <Greeting name="Bob" />
    </View>
  );
};
```

## 3. State (Mutable)

State allows a component to change its own data over time (e.g., user input).
Use the `useState` hook.

```jsx
import React, { useState } from "react";
import { Button, Text, View } from "react-native";

const Counter = () => {
  // [currentValue, setterFunction] = useState(initialValue)
  const [count, setCount] = useState(0);

  return (
    <View>
      <Text>Count: {count}</Text>
      <Button title="Increment" onPress={() => setCount(count + 1)} />
    </View>
  );
};
```

## Key Takeaways

- **Props**: Passed _in_. Cannot be changed by the child.
- **State**: Managed _within_. Triggers a re-render when changed.
- **Hooks**: `useState` must be called at the top level of the component function.
