# Lesson 3: Accessibility

## Learning Goals

- Support Screen Readers (TalkBack / VoiceOver).
- Use Accessibility Props.

## 1. Why it matters

Mobile apps use gestures that are hard for some users. Screen readers rely on semantic data.

## 2. Core Props

- `accessible`: (Boolean) Group elements into a single focusable component.
- `accessibilityLabel`: The text read aloud by the screen reader.
- `accessibilityRole`: Describes the component (e.g., 'button', 'header').

```jsx
<Pressable
  accessible={true}
  accessibilityRole="button"
  accessibilityLabel="Tap me to login"
  onPress={handleLogin}
>
  <Text>Login</Text>
</Pressable>
```

## 3. Testing

- **Simulator**: Use the "Accessibility Inspector" (Mac).
- **Device**: Turn on VoiceOver (iOS) or TalkBack (Android) in Settings.

## Key Takeaways

- Unlike Web (where you use semantic HTML `<button>`), in React Native requires you to explicitely define **Roles**.
- Images MUST have labels if they convey meaning.
