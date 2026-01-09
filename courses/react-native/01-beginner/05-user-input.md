# Lesson 5: Handling User Input

## Learning Goals

- Use `TextInput`.
- Handle Buttons and Touchables.
- Manage Keyboard visibility.

## 1. TextInput

Controlled components are the standard.

```jsx
const LoginScreen = () => {
  const [email, setEmail] = useState("");

  return (
    <TextInput
      style={styles.input}
      placeholder="Enter Email"
      value={email}
      onChangeText={(text) => setEmail(text)} // Directly gives the text string
      keyboardType="email-address"
      autoCapitalize="none"
    />
  );
};
```

- **Diff to Web**: It's `onChangeText`, not `onChange`. You get the string directly, not an event object.

## 2. Buttons vs Pressable

- `<Button />`: Native look (Blue text on iOS, Blue rectangle on Android). Limited styling.
- `<Pressable>`: Fully customizable. Use this for custom UI.

```jsx
import { Pressable, Text } from "react-native";

<Pressable
  onPress={() => console.log("Pressed!")}
  style={({ pressed }) => [
    styles.button,
    pressed && styles.buttonPressed, // Change opacity on press
  ]}
>
  <Text>Login</Text>
</Pressable>;
```

## 3. KeyboardAvoidingView

The virtual keyboard often covers input fields.
Wrap your form in this.

```jsx
<KeyboardAvoidingView
  behavior={Platform.OS === "ios" ? "padding" : "height"}
  style={styles.container}
>
  {/* Inputs go here */}
</KeyboardAvoidingView>
```

## Key Takeaways

- Use **Pressable** for custom buttons.
- Always test `TextInput` with the keyboard open to ensure visibility.
- **Platform Check**: Keyboard behavior varies wildly between iOS and Android.
