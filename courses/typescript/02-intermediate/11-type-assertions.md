# 11. Type Assertions (as)

## 🎯 Learning Goal

Override TypeScript's inference.

## 🧠 Concept

Sometimes you know more than the compiler.
`value as Type`.

## 💻 Implementation

### Basic Assertion

```typescript
const myCanvas = document.getElementById("main_canvas") as HTMLCanvasElement;
```

### const Assertion

Locks a value as a literal.

```typescript
const config = {
  method: "GET" as const, // Type is "GET", not string
  timeout: 5000,
};
```

### Non-null Assertion (!)

Tells TS "Trust me, this isn't null".

```typescript
function live(x?: number) {
  console.log(x!.toFixed()); // Crash if x is undefined!
}
```

## 🧩 Activity / Challenge

1.  Try to cast `string` to `number`. TS blocks it.
2.  Force it: `x as unknown as number` (Double casting). Discuss why this is dangerous.

## 🔑 Key Takeaways

- Use assertions sparingly. If you lie to the compiler, runtime errors will return.
