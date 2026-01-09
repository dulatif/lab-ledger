# Lesson 9: Publishing

## Learning Goals

- Keystore / Provisioning Profile.
- App Stores.

## 1. Android

1.  Create `upload-keystore.jks`.
2.  Create `key.properties`.
3.  Update `android/app/build.gradle` to use the keystore.
4.  `flutter build appbundle`.

## 2. iOS

1.  Register App ID in Apple Developer Console.
2.  Open `ios/Runner.xcworkspace` in Xcode.
3.  Set "Signing & Capabilities".
4.  `flutter build ipa`.

## 3. Shorebird (Code Push)

Optional: Push updates to Dart code _without_ a store release.
`shorebird release android`.

## Key Takeaways

- Keep your Keystore safe! If you lose it, you can't update your app.
- Review guidelines (Human Interface Guidelines) to avoid rejection.
