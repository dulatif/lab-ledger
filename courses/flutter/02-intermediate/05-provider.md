# Lesson 5: InheritedWidget & Provider

## Learning Goals

- Lifting State Up.
- Dependency Injection (DI).
- `Provider` package.

## 1. The Core Problem

Passing data 10 layers down (`prop drilling`) is bad.
`InheritedWidget` allows O(1) access to data up the tree.

## 2. Provider

A wrapper around `InheritedWidget`. The community standard.

**Setup**:

```dart
void main() {
  runApp(
    ChangeNotifierProvider(
      create: (context) => CounterModel(),
      child: MyApp(),
    ),
  );
}
```

**Consumption**:

```dart
final count = context.watch<CounterModel>().count;
// or
Consumer<CounterModel>(builder: (context, model, child) {
  return Text(model.count.toString());
});
```

## Key Takeaways

- `Provider` separates Logic (Model) from UI (Widget).
- It's the foundation for more advanced tools (Riverpod).
