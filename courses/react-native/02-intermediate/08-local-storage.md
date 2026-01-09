# Lesson 8: Local Storage

## Learning Goals

- Persist simple data key-value pairs.
- Understand async storage.

## 1. AsyncStorage

The LocalStorage of mobile.
**Note**: It is unencrypted. Do NOT store passwords or auth tokens here (use SecureStore for that).

```bash
npx expo install @react-native-async-storage/async-storage
```

**Usage**:

```jsx
import AsyncStorage from "@react-native-async-storage/async-storage";

// Saving (Value must be String)
const saveSettings = async (value) => {
  try {
    await AsyncStorage.setItem("@theme", value);
  } catch (e) {
    // saving error
  }
};

// Reading
const getSettings = async () => {
  try {
    const value = await AsyncStorage.getItem("@theme");
    if (value !== null) {
      // value previously stored
    }
  } catch (e) {
    // error reading value
  }
};
```

## 2. SecureStore (Expo)

For sensitive data (like API Keys).

```jsx
import * as SecureStore from "expo-secure-store";

await SecureStore.setItemAsync("secure_token", "abc-123");
```

## Key Takeaways

- **AsyncStorage**: Simple, persistent, unencrypted JSON storage. Good for "User Preferences" (Dark Mode).
- **SecureStore**: Encrypted storage. Good for "Auth Tokens".
