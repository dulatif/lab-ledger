# Lesson 3: Primitives & Arrays

## Learning Goals

- Basic Types.
- Arrays.

## 1. Primitives

Same as JS:

- `string`: "hello"
- `number`: 42, 3.14
- `boolean`: true, false

```typescript
let age: number = 25;
let name: string = "Alice";
```

## 2. Arrays

Two syntaxes:

1.  `string[]`: Array of strings.
2.  `Array<string>`: Generic syntax.

```typescript
const scores: number[] = [90, 85, 100];
```

## 3. Type Inference

TS is smart. You don't usually need to write the type.
`let x = 10;` is inferred as `number`.

## Key Takeaways

- Trust inference. Only annotate when necessary (e.g., function arguments).
