# Lesson 9: Deployment (App Stores)

## Learning Goals

- Configure `app.json`.
- Use EAS Build (Expo Application Services).
- Submit to Stores.

## 1. Configuration (`app.json`)

Before building, set your identity.

- `bundleIdentifier` (iOS): `com.yourname.appname` (Must be unique on Apple).
- `package` (Android): `com.yourname.appname` (Must be unique on Play Store).
- `version`: `1.0.0`.
- `icon`: Your app icon (1024x1024).

## 2. EAS Build

Expo's cloud build service. You don't need a Mac to build an iOS app!

```bash
npm install -g eas-cli
eas login
eas build:configure
```

**Run a Build**:

```bash
eas build --platform ios --profile production
```

This uploads your code -> Compiles IPA/AAB in the cloud -> Gives you a download link.

## 3. Submission

- **Android**: Upload `.aab` file to Google Play Console.
- **iOS**: Use **EAS Submit** or Transporter app to upload `.ipa` to App Store Connect.

## 4. Updates (OTA)

**EAS Update** allows you to fix JavaScript bugs (typos, logic) instantly without going through the App Store review process!

```bash
eas update --branch production --message "Fix login bug"
```

Users will download the new JS bundle silently on next launch.

## Key Takeaways

- **EAS Build** is magic. It saves hours of configuration.
- **OTA Updates**: A superpower of React Native. Use it responsibly.
- **Icons/Splashes**: Expo handles resizing automatically.
