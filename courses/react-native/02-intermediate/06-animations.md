# Lesson 6: Animations

## Learning Goals

- Use the `Animated` API.
- Understand `useNativeDriver`.

## 1. The Animated API

React Native has a powerful declarative animation system.

```jsx
import React, { useRef, useEffect } from "react";
import { Animated, Text, View } from "react-native";

const FadeInView = (props) => {
  const fadeAnim = useRef(new Animated.Value(0)).current; // Initial Value: 0

  useEffect(() => {
    Animated.timing(fadeAnim, {
      toValue: 1, // Final Value: 1
      duration: 1000,
      useNativeDriver: true, // IMPORTANT!
    }).start();
  }, [fadeAnim]);

  return (
    <Animated.View style={{ ...props.style, opacity: fadeAnim }}>
      {props.children}
    </Animated.View>
  );
};
```

## 2. Native Driver

Always set `useNativeDriver: true`.
This offloads the animation work to the native UI thread.
If `false`, the JS thread calculates every frame -> dropping frames if JS is busy.

**Limitation**: Only transform properties (scale, rotate, translate, opacity) work with Native Driver. Changing `width` or `height` requires `useNativeDriver: false`.

## 3. React Native Reanimated

For gesture-driven complex animations (drag and drop), the community standard is **React Native Reanimated** (library), but `Animated` API is fine for simple fades/slides.

## Key Takeaways

- **Animated.Value**: The mutable number that drives the animation.
- **useNativeDriver**: Always use strictly true for performance.
