# Lesson 3: Local Storage

## Learning Goals

- `shared_preferences`.
- Unstructured data vs Structured data.

## 1. Shared Preferences

Best for simple flags (Theme Mode, Login Token, Onboarding seen).
It stores Key-Value pairs.

```yaml
dependencies:
  shared_preferences: ^2.2.0
```

```dart
import 'package:shared_preferences/shared_preferences.dart';

// Save
final prefs = await SharedPreferences.getInstance();
await prefs.setInt('counter', 10);

// Read
final int? counter = prefs.getInt('counter');
```

## 2. SQLite (sqflite)

For structured, queryable data (Offline Todo list).
It is more complex to set up.

```dart
// Open DB
final db = await openDatabase('my_db.db');

// Insert
await db.insert('dogs', {'name': 'Fido', 'age': 3});

// Query
final List<Map> maps = await db.query('dogs');
```

## Key Takeaways

- Don't use SharedPrefs for huge datasets.
- Use SQLite or Hive for complex offline data.
