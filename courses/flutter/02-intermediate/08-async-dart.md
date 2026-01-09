# Lesson 8: Asynchronous Dart

## Learning Goals

- The Event Loop.
- Streams.
- Generators.

## 1. The Event Loop

Dart is single-threaded.
`Future` puts a task in the "Event Queue" to run later.
This prevents the UI from freezing.

## 2. Streams

A sequence of asynchronous events. (Like a pipe of water).
`Future` return 1 value. `Stream` returns N values.

```dart
Stream<int> countStream() async* {
  for (int i = 1; i <= 5; i++) {
    await Future.delayed(Duration(seconds: 1));
    yield i; // "Return" a value but keep function alive
  }
}

// Usage
StreamBuilder<int>(
  stream: countStream(),
  builder: (context, snapshot) => Text('${snapshot.data}'),
);
```

## 3. StreamController

To manually add items to a stream.

```dart
final controller = StreamController<String>();
controller.add('Hello');
controller.stream.listen((val) => print(val));
```

## Key Takeaways

- Close your Streams/Controllers (`dispose()`)! Or you will leak memory.
- Streams are the backbone of BLoC.
