# Lesson 2: React Native Web

## Learning Goals

- Run React Native code in a Browser.
- Understand the limitations.

## 1. How it works

`react-native-web` is a library (used by Expo by default) that maps standard Native components to HTML/CSS.

- `<View>` -> `<div>` with Flexbox.
- `<Text>` -> `<span>` with specific classes.
- `<Image>` -> `<img>`.

## 2. Platform Checking for Web

Web is just another platform.

```jsx
import { Platform } from "react-native";

if (Platform.OS === "web") {
  // Do web specific stuff, like window.location
}
```

## 3. Limitations

- **Navigation**: Mobile navigation (Stacks) feels weird on Web. You usually need separate routing logic or strict responsive design.
- **Native Modules**: Things like `Camera` or `Haptics` might not work or behave differently on the web. Always check library compatibility.

## Key Takeaways

- **Expo** runs on Web out of the box.
- It is great for **PWAs** (Progressive Web Apps) or Admin panels.
- Ideally, reuse **business logic**, but create separate **Layouts** for Web vs Mobile if the UX differs significantly.
