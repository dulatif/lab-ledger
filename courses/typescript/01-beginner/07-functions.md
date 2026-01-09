# Lesson 7: Typing Functions

## Learning Goals

- Arguments.
- Return Types.

## 1. The Syntax

Always type arguments. Return type is often inferred.

```typescript
function add(a: number, b: number): number {
  return a + b;
}
```

## 2. Function Types

You can define a type for a function itself (callbacks).

```typescript
type MathOp = (x: number, y: number) => number;

const multiply: MathOp = (x, y) => x * y;
```

## Key Takeaways

- `noImplicitAny` forces you to type inputs.
- Return type annotations help prevent you from returning the wrong thing accidentally.
