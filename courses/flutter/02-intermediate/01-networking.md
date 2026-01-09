# Lesson 1: Networking (HTTP)

## Learning Goals

- The `http` package.
- REST API basics.
- Async/Await.

## 1. Setup

Add `http` to `pubspec.yaml`.

```yaml
dependencies:
  http: ^1.2.0
```

## 2. Making a Request

Network calls are asynchronous. We use `Future` and `await`.

```dart
import 'package:http/http.dart' as http;

Future<void> fetchData() async {
  final url = Uri.parse('https://jsonplaceholder.typicode.com/posts/1');
  final response = await http.get(url);

  if (response.statusCode == 200) {
    print('Response body: ${response.body}');
  } else {
    print('Request failed with status: ${response.statusCode}.');
  }
}
```

## 3. displaying Data (FutureBuilder)

`FutureBuilder` handles the 3 states of a Future: Loading, Data, Error.

```dart
FutureBuilder<String>(
  future: fetchData(), // Your async function
  builder: (context, snapshot) {
    if (snapshot.hasData) {
      return Text(snapshot.data!);
    } else if (snapshot.hasError) {
      return Text('Error: ${snapshot.error}');
    }
    return CircularProgressIndicator();
  },
);
```

## Key Takeaways

- Never block the main thread. Always use `async`/`await` for I/O.
- `FutureBuilder` is great for simple one-off fetches.
