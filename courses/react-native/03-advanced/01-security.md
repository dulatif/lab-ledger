# Lesson 1: Security Best Practices

## Learning Goals

- Secure Data Storage.
- Prevent Man-in-the-Middle (MITM) attacks.
- Secure Deep Linking.

## 1. Secure Storage

**Never** store sensitive info (Passwords, Auth Tokens, Credit Card info) in `AsyncStorage` or Redux State.
Use **SecureStore** (Expo) or `react-native-keychain`.

- **iOS**: Uses Keychain Services.
- **Android**: Uses Keystore system.

## 2. Network Security (SSL Pinning)

HTTPS is standard, but hackers can still install a custom root certificate on a user's phone to inspect traffic (MITM).
**SSL Pinning** forces the app to trust _only_ your specific server certificate, rejecting all others.

- **Bare React Native**: Use `react-native-ssl-pinning`.
- **Expo**: Requires Config Plugins to modify native code.

## 3. Biometrics

Use `expo-local-authentication` to require FaceID/Fingerprint before accessing sensitive screens (e.g., "Show API Key").

```jsx
import * as LocalAuthentication from "expo-local-authentication";

const authenticate = async () => {
  const hasHardware = await LocalAuthentication.hasHardwareAsync();
  if (hasHardware) {
    const result = await LocalAuthentication.authenticateAsync();
    if (result.success) {
      // Show sensitive data
    }
  }
};
```

## Key Takeaways

- **AsyncStorage** = Public data only.
- **Biometrics** adds a layer of security for sensitive actions.
- **Obfuscation**: JavaScript is readable. ProGuard (Android) shrinks code, but logic can still be reversed. Don't put API Secrets in your code!
