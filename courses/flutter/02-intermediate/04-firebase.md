# Lesson 4: Cloud Integration (Firebase)

## Learning Goals

- FlutterFire.
- Authentication.
- Firestore.

## 1. FlutterFire CLI

The easiest way to connect.

1.  Install CLI: `dart pub global activate flutterfire_cli`.
2.  Configure: `flutterfire configure`.
    This generates `firebase_options.dart`.

## 2. Initialization

```dart
import 'package:firebase_core/firebase_core.dart';
import 'firebase_options.dart';

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp(
    options: DefaultFirebaseOptions.currentPlatform,
  );
  runApp(MyApp());
}
```

## 3. Usage

**Auth**:

```dart
await FirebaseAuth.instance.signInWithEmailAndPassword(
  email: 'barry.allen@example.com',
  password: 'SuperSecretPassword!'
);
```

**Firestore**:

```dart
FirebaseFirestore.instance
    .collection('users')
    .add({'full_name': 'Barry Allen'});
```

## Key Takeaways

- Firebase is the "Standard Library" for Flutter backends.
- `flutterfire configure` keeps your config in sync with the console.
