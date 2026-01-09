# Lesson 6: Riverpod & BLoC

## Learning Goals

- Riverpod (Provider 2.0).
- BLoC (Streams).

## 1. Riverpod

Solves Provider's issues (Runtime errors, Context dependency).
Providers are declared globally.

```dart
final counterProvider = StateProvider((ref) => 0);

class Home extends ConsumerWidget {
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final count = ref.watch(counterProvider);
    return Text('$count');
  }
}
```

## 2. BLoC (Business Logic Component)

Enterprise standard. Strict separation via **Events** (Input) and **States** (Output).
Uses Streams.

```dart
// Event: Increment
// State: 1 -> 2

class CounterBloc extends Bloc<CounterEvent, int> {
  CounterBloc() : super(0) {
    on<Increment>((event, emit) => emit(state + 1));
  }
}
```

## Key Takeaways

- **Riverpod**: great for simple to medium apps. Flexible.
- **BLoC**: great for strict, large teams. Verbose but predictable.
