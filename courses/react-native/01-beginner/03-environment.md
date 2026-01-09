# Lesson 3: Environment Setup (Expo)

## Learning Goals

- Understand Expo Go.
- Run your first app.

## 1. Why Expo?

- **React Native CLI**: The "pure" way. Requires installing Android Studio (10GB+) and Xcode (Mac only). Complex to upgrade.
- **Expo**: The "managed" framework. Build apps without installing build tools initially. Run on your physical phone instantly via QR code.

**We will use Expo for this course.** It is the industry standard for starting new apps.

## 2. Prerequisites

1.  **Node.js**: Install the LTS version.
2.  **Expo Go App**: Install this app from the App Store / Play Store on your **physical phone**.

## 3. Create & Run

Open your terminal:

```bash
# 1. Create the project
npx create-expo-app MyFirstApp

# 2. Navigate in
cd MyFirstApp

# 3. Start the bundler
npx expo start
```

You will see a QR Code in the terminal.

- **iPhone**: Open Camera app -> Scan QR -> Opens in Expo Go.
- **Android**: Open Expo Go app -> Scan QR.

## 4. Troubleshooting

- **Network Error**: Your phone and computer MUST be on the **same Wi-Fi**.
- **Metro Bundler**: This is the process running in your terminal. It compiles JS code and sends it to the phone. Do not close the terminal!

## Key Takeaways

- **Expo Go** lets you test native code without compiling a standard APK/IPA.
- **Hot Reload**: Save a file in your editor, and the phone updates instantly.
