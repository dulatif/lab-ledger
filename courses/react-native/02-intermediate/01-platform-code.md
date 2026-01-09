# Lesson 1: Platform Specific Code

## Learning Goals

- Use `Platform.OS`.
- Use `Platform.select`.
- Use File Extensions (`.ios.js`, `.android.js`).

## 1. The Platform Module

Sometimes you need different values for iOS and Android (e.g., Font families, Padding).

```jsx
import { Platform, StyleSheet } from "react-native";

const styles = StyleSheet.create({
  header: {
    paddingTop: Platform.OS === "ios" ? 50 : 20,
  },
});
```

## 2. Platform.select

A cleaner way to write objects that differ by platform.

```jsx
const buttonColor = Platform.select({
  ios: "blue",
  android: "green",
  default: "black", // Web or others
});
```

## 3. Platform-Specific Files

If the logic is vastly different (e.g., specific native modules), split them into separate files.

- `BigButton.ios.js`
- `BigButton.android.js`

Import it normally:
`import BigButton from './BigButton';`
React Native automatically picks the correct one during the build process.

## Key Takeaways

- Use **Platform.OS** for simple logic checks.
- Use **Platform.select** for configuration objects.
- Use **File Extensions** for differing component structures.
