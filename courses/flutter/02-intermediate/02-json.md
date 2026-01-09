# Lesson 2: JSON & Serialization

## Learning Goals

- `dart:convert`.
- Data Models.
- `fromJson`.

## 1. Decoding JSON

`jsonDecode` turns a String into a `Map<String, dynamic>`.

```dart
import 'dart:convert';

final jsonString = '{"id": 1, "title": "Hello"}';
final Map<String, dynamic> userMap = jsonDecode(jsonString);
print(userMap['title']);
```

## 2. Model Classes

Working with raw Maps is error-prone. Use typed Objects.

```dart
class Post {
  final int id;
  final String title;

  const Post({required this.id, required this.title});

  // Factory constructor for JSON deserialization
  factory Post.fromJson(Map<String, dynamic> json) {
    return Post(
      id: json['id'] as int,
      title: json['title'] as String,
    );
  }

  // Method for serialization
  Map<String, dynamic> toJson() {
    return {
      'id': id,
      'title': title,
    };
  }
}
```

## 3. Automation

For large apps, use `json_serializable` and `build_runner` to generate this boilerplate automatically.

## Key Takeaways

- Always parse JSON into strongly-typed Models at the network boundary.
- Use `factory` constructors for instantiation logic.
