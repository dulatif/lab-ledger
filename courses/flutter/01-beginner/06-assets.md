# Lesson 6: Assets & Resources

## Learning Goals

- `pubspec.yaml`.
- Images.
- Fonts.

## 1. pubspec.yaml

The manifest file. Indentation (2 spaces) is critical!

```yaml
flutter:
  assets:
    - assets/images/logo.png
    - assets/data.json
```

## 2. Displaying Images

```dart
// Local Asset
Image.asset('assets/images/logo.png');

// Network Image
Image.network('https://example.com/pic.jpg');
```

## 3. Custom Fonts

1.  Download font (e.g., `.ttf`).
2.  Put in `assets/fonts/`.
3.  Register in `pubspec.yaml`:

```yaml
fonts:
  - family: MyFont
    fonts:
      - asset: assets/fonts/MyFont-Regular.ttf
```

4.  Use it: `TextStyle(fontFamily: 'MyFont')`.

## Key Takeaways

- Always run `flutter pub get` after changing `pubspec.yaml`.
- Keep your assets folder organized.
